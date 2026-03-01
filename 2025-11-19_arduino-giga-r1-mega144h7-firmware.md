# Arduino Giga R1 (STM32H747XI) with Mega144H7 Firmware

*Source: rusEFI Discord, 2025-11-19 | Channel: 1401895238481744052 and 1356732771325968630*
*Contributors: @mykkh, @ggurov*

## Summary

A user experimenting with the Arduino Giga R1 development board (which uses the STM32H747XI dual-core MCU — the same chip as ggurov's epicEFI ECUs) asked which rusEFI/epicEFI firmware variant to use. The confirmed answer is Mega144H7. A separate experiment loading EpicFW 11-16 onto the Giga R1 showed it communicates with TunerStudio normally but cannot drive the UA4C output driver module, likely due to incorrect pin mapping for that board.

## Details

### Hardware: Arduino Giga R1

- MCU: **STM32H747XI** (dual-core: Cortex-M7 at 480MHz + Cortex-M4 at 240MHz)
- This is the same MCU used in ggurov's epicEFI ECU hardware
- The Arduino Giga R1 is a development/hobbyist board in Mega form factor

### Correct Firmware Variant

- **Mega144H7** is the confirmed correct firmware for the Arduino Giga R1
- @ggurov: "Mega144H7 is correct — unnamed pins, exposes actual ports"
- The "unnamed pins" / "exposes actual ports" description means this firmware variant does not assume a fixed pin-to-function mapping; instead it exposes raw MCU port assignments, allowing the user to configure each physical pin manually
- This is appropriate for non-standard boards where the pin connections differ from the standard Mega layout

### EpicFW 11-16 Experiment on Giga R1

- @mykkh loaded EpicFW version 11-16 onto the Giga R1
- Result: firmware loaded successfully, communicates with TunerStudio normally
- Problem: **UA4C output driver module outputs do not activate**
- Probable cause: EpicFW 11-16 is built for a specific epicEFI PCB with a different pinout than the Arduino Giga R1; the UA4C output channels are mapped to different MCU pins than what the Giga R1 exposes, so the driver signals never reach the UA4C chip
- @ggurov acknowledged this: "prolly not the right pinout"

### Dual-Core Usage

- @mykkh asked whether the dual-core nature of the H747 is better served by a specific firmware variant
- Mega144H7 is appropriate — the firmware runs on the M7 core; M4 core usage depends on the specific firmware build
- @mitch0s (in the same discussion) recommended implementing a HAL middle layer that configures which tasks run on each core, handling inter-core communication via soft interrupts (e.g., PendSV on ARM)
