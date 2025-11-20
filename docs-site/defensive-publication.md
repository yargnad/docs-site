# Defensive Publication: Sensor-Fusion-to-Audio Methods

Anima Locus publishes a concise, citable summary of our sensor-fusion methods to establish prior art and protect the community from patent capture. This file is an overview, not a detailed code dump; concrete implementation may remain in separate repos.

## Summary

- Goal: Document core mathematical ideas and high-level architecture for mapping environmental sensors and gestural data to audio engine parameters in a way that demonstrates novelty and prior use.

## Key Concepts

1. Feature frames: Compact, timestamped feature vectors containing fused sensor outputs (radar presence vectors, E-field proximity vector, ToF depth images reduced to centroid/density, and environment deltas for humidity/CO2/temperature).
2. Hysteretic mapping: Environmental streams are treated with long-slew (30–120s) smoothing kernels combined with hysteresis (dual-threshold step responses) to avoid large-bounce responses.
3. Terroir bus: A normalized, bounded set of macros (e.g., spectral tilt, tube bias, grain density, envelope curvature) that receives the fused environmental data and can be mixed by a single "Terroir Depth" control.
4. Deterministic link: Frames generated within an STM32 microcontroller at fixed cadences and transported to the audio engine using ring buffers or shared RAM to achieve sub-10ms gesture latency.

## Licensing

This document is intended as a defensive publication to establish prior art and is published under CC BY-SA 4.0 for the content.

## Notes

- This is an executive-level defense document. Implementation specifics are in the corresponding repositories and follow AGPLv3/CERN-OHL-S v2 as appropriate.
