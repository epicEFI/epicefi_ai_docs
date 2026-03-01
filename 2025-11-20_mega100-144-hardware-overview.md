# MEGA100/144 and M144 Hardware: STM32 on Arduino Mega2560 Form Factor

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: Tera (@teraflopping), Joesphan (@joesphan)*

## Summary

The MEGA100, MEGA144, and M144 are epicEFI ECU hardware variants that use an STM32 microcontroller mounted on a board with the physical form factor of an Arduino Mega2560. The "M" prefix stands for MEGA. These boards are distinct from the Proteus (which uses STM32F4 or F7) and represent the earlier epicEFI hardware lineage.

## Details

**Naming convention:**
- "M144" and "MEGA100/144" refer to the same hardware family.
- The "M" prefix is shorthand for "MEGA" — a board that physically fits the Arduino Mega2560 footprint.
- These are ggurov's boards ("that's gurov's board" — Joesphan).
- "epic before it was epic" — Joesphan describing the lineage as the predecessor hardware to the named epicEFI product.

**MCU:**
- Uses an STM32 microcontroller on a Mega2560-compatible form factor.
- Allows drop-in physical compatibility with existing Mega2560 hardware designs and shields.

**Firmware builds:**
- The firmware build list (including which targets are supported) is available on the epicEFI content/docs page.
- The Proteus F4 variant is confirmed to run epicEFI firmware.
- The "M144" / MEGA form factor targets are separate builds from the Proteus family.

**Relevance:**
- When selecting a firmware binary, users should identify their specific board variant (Proteus F4, Proteus F7, MEGA/M144, etc.) because the firmware builds are hardware-specific.
- The Mega2560 form factor boards also serve as expansion hardware (see related doc on Mega2560 I/O expansion).
