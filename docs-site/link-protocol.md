# Link Protocol — MCU ↔ Linux

This page documents the core link protocol, framing, and timing requirements for deterministic, low‑latency sensor delivery between the MCU (`mcu-stm32`) and the Linux audio engine (`engine-ui`). This is a defensive, readable specification and a testable contract for implementers.

## Goals

- Provide a compact, deterministic framing format for sensor/gesture/feature messages
- Keep jitter bounded (< 10 ms end-to-end for gesture-to-parameter paths)
- Support both UART/CDC and SPI transports; prefer DMA for bulk transfers
- Offer a compact `elemental` bus message for expressive mapping
- Support ring-buffer/shared-RAM transfer when exposed by the carrier board

---
## Framing (binary)

All messages use this framing when transported over UART/CDC or any stream-style transport.

Field layout:
```
[SYNC:2][TYPE:1][SEQ:2][LEN:2][TS:4][PAYLOAD:LEN][CRC16:2]
```
- SYNC: 0xAA 0x55 (2 bytes)
- TYPE: 1 byte message type (see table)
- SEQ: 16-bit monotonic sequence number, wraps at 0xFFFF
- LEN: length of PAYLOAD in bytes (16-bit)
- TS: 32-bit monotonic timestamp in ms (host or MCU monotonic)
- PAYLOAD: variable
- CRC16: CRC-16-CCITT (0x1021) over TYPE..PAYLOAD

### Message types
- 0x01 SENSOR_SNAPSHOT — env sensors; payload: env ID + scaled integers or packed floats
- 0x02 GESTURE_EVENT — classifier id, confidence, bounding boxes/position (compact)
- 0x03 RADAR_FRAME — minimal set of tracked objects: N, (x,y,z,v) list
- 0x04 EFIELD_EVENT — approach/release, magnitude
- 0x10 INFERENCE_RESULT — model_id + bytes embedding
- 0x20 ELEMENTAL_BUS — 4 signed bytes encoding Earth, Air, Water, Fire [-127..127]
- 0xF0 HEARTBEAT — small payload for liveness
- 0xFF DIAGNOSTIC — debug payload

### Example: `ELEMENTAL_BUS` payload
```
[earth:int8][air:int8][water:int8][fire:int8]
```
Normalized in Linux by mapping to floats: value/127.0 → [-1.0..1.0]

---
## Transport notes
- UART/CDC: use DMA on MCU for reads and writes; set high priority for RX
- SPI: full-duplex mode; use DMA for bulk writes and a short command/ack handshake for control
- Shared RAM / ring-buffer: if the carrier exposes ring memory, use a lockless producer/consumer pattern with sequence numbers and a write barrier. If not available, SPI + DMA or UART + DMA are fine.

---
## Example ring-buffer contract (shared RAM)
```
struct ringbuf_hdr {
  uint32_t size; // buffer size
  uint32_t head; // producer index
  uint32_t tail; // consumer index
  uint32_t seq;  // monotonic sequence number
};

// Producer writes frame at head % size, copies header->size bytes, increments head and seq.
// Consumer reads until head==tail. Use release/consume memory barriers.
```
- Lay out as small as possible and keep buffers power-of-two size.
- Prefer 64-bit aligned headers to avoid partial writes.

---
## Timing & Cadence
- Gesture frames: high cadence, up to 2000 Hz for raw gestures; typically processed at 60-500 Hz after MCU preprocessing.
- Environmental frames: low cadence (1-10 Hz)
- Elemental bus: 10-60 Hz
- End-to-end goal: <10ms latency from gesture detection to engine parameter application

Test harness should measure round trip jitter; see `engine-ui/tools/link_bench.py` for an example.

---
## Error handling
- CRC errors: discard message and log; increment diagnostics counter
- Sequence gaps: fill missing frames by interpolating last known values and log missing frames
- Diagnostic message: `0xFF` used to transmit human-readable or encoded error codes

---
## Example Python parser (pseudocode)
```python
import serial
import struct
import time

ser = serial.Serial('/dev/ttyACM0', 115200)

# sync search + framing
while True:
    b1 = ser.read(1)
    if b1 == b"\xaa":
        b2 = ser.read(1)
        if b2 == b"\x55":
            head = ser.read(1+2+2+4) # type+seq+len+ts
            typ, seq, length, ts = struct.unpack('<BHLl', head)
            payload = ser.read(length)
            crc = ser.read(2)
            # validate CRC
            # decode payload etc.
```

---
## Next steps / tests
- Implement `mcu-stm32/bench/elemental_producer` that publishes `ELEMENTAL_BUS` at 30 Hz
- Implement `engine-ui/tools/link_bench.py` to consume and measure jitter
- Add OpenAPI & WebSocket docs for `elements` messages
- Add CI job in `engine-ui` that runs a small bench in Docker or a simulated serial loop

This is an initial spec; it is intentionally conservative and test-first. Implementations should add test vectors and unit tests to validate framing and CRC.
