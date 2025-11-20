# Elemental Controls

Elemental Controls are a small set of expressive buses used across the Anima Locus system to provide broadly musical, low‑cognitive‑load mappings between sensor fusion data and audio/visual parameters.

## Definition

Elemental controls are four named buses:
- Earth — density/low-frequency weight
- Air — spectral brightness/high-shelf
- Water — spatial size, reverb diffusion, and decay
- Fire — drive, harmonic saturation, distortion

## Why Elemental Controls?

- Simplifies mappings for performers and visual artists
- Provides a stable, documented contract for SDKs and the HUD
- Enables a consistent "terroir" bus that maps environmental readings into artistic parameters

## Implementation Notes

- Values are normalized to [-1.0, +1.0] or to [0.0, 1.0] depending on engine convenience; SDKs provide a canonical mapping.
- Element values can be exported to ER Files for replay and analysis.
- The MCU may publish an elemental bus derived from tinyML feature outputs or fusion heuristics; see `mcu-stm32/README.md` for the binary format.

## Example Mappings

- CO₂ or occupancy increases → Earth (density)
- Ambient light increases → Air (brilliance)
- Humidity increases → Water (wetness/reverb)
- Temperature/presence → Fire (drive)

## API

WebSocket messages and SDK functions are documented in `engine-ui/` and `sdk-*` packages.
