# epicECU SPI Exhaust Gas Temperature (EGT) Channel Count

*Source: rusEFI Discord, 2026-02-20 | Channel: 1356732771325968630*
*Contributors: @ggurov*

## Summary

The epicECU hardware supports 8 SPI-connected Exhaust Gas Temperature (EGT) channels, enabling per-cylinder or per-bank EGT monitoring on multi-cylinder engines.

## Details

### SPI EGT Channel Count

- The epicECU has **8x SPI EGT channels**.
- EGT sensors communicate over SPI (Serial Peripheral Interface), allowing multiple sensors to be connected without requiring individual analog inputs.
- This count was confirmed by @ggurov, who is a primary developer of the epicEFI platform.

### Typical Use Cases

- Per-cylinder EGT monitoring on 4-, 6-, or 8-cylinder engines.
- Turbo inlet/outlet temperature monitoring.
- Exhaust manifold heat balancing for tuning purposes.

SPI-based EGT typically uses thermocouple amplifier ICs (such as the MAX31855 or similar) which communicate over SPI and can be chained or chip-select multiplexed to use the 8 available channels.
