# STM32 MCU Drop-In Compatibility: F4, F7, and H7 on epicEFI/rusEFI

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ggurov, @YeOldePirate, @mke, @Joesphan*

## Summary

The epicEFI/rusEFI firmware supports STM32F4, STM32F7, and STM32H7 microcontrollers and, on ggurov's custom ECU hardware, the 144-pin variants of these MCUs are physically pin-compatible drop-in replacements for one another. The firmware currently fits all three MCU families. The primary epicEFI platform targets the F7 series. The F4 (100-pin) has reduced RAM compared to the 144-pin variants, but the current firmware still fits with reasonable table sizes. The 100-pin F7 variant has some GPIO remapping that introduces complications and is not recommended.

## Details

### Supported MCU Families

The current firmware compiles and runs on:
- **STM32F4** (e.g., STM32F407VGT6)
- **STM32F7** (e.g., STM32F767ZGT6) — primary target for epicEFI
- **STM32H7** (e.g., STM32H743VIT6)

> "so far, the firmware fits into f4 f7 h7" — @ggurov

@Joesphan noted: "I believe gurov has a branch for f4 and f7" and confirmed his own car's alpha ECU runs epicEFI on F4.

### 144-Pin Drop-In Compatibility

On ggurov's socket-based ECU hardware (designed around a Mega2560-style socket), the following 144-pin MCUs are physically pin-compatible and can be swapped without board changes:

| Part Number | MCU Series |
|-------------|-----------|
| STM32F407VGT6 | F4 |
| STM32F767ZGT6 | F7 |
| STM32H743VIT6 | H7 |

> "on my ecus, f4 f7 h7 are all drop in" — @ggurov
> "the 144 f407vgt6, f767zgt6 and h743vit6 are" — @ggurov (confirming 144-pin specifically)
> "cause mega2560 socket based" — @ggurov (explaining the board design)

The hardware is also compatible with Speeduino firmware.

### Important Caveat: 144-Pin vs 100-Pin

Drop-in compatibility applies **only to the 144-pin package variants**. The 100-pin F7 variant is **not** a straightforward drop-in:

> "it has to be 144 pin" — @ggurov
> "f7 in 100 pin has jank" — @ggurov

The 100-pin F7 has some GPIO pins remapped compared to the 144-pin version. Additionally, the 100-pin F4 has half the RAM of the 144-pin version, which limits the available table sizes.

@YeOldePirate's reaction upon learning this while working on a 100-pin board design: "I AM NOT REDESIGNING THE BOARD A THIRD TIME" — ultimately deciding the F407 (100-pin) was sufficient for a basic fuel-only setup.

### Table Size on F4

Despite lower RAM, the firmware fits the F4 with usable table dimensions:

> "32x32 fuel, 20x20 ign fits — with all the other stuff" — @ggurov

The 16x16 main table limitation that existed in stock rusEFI was resolved in epicEFI (see separate doc on 16x16 table fix).

### MCU Selection Tool

There is a tool (STM32CubeMX) that can be used to check GPIO compatibility between MCU variants before committing to a hardware design:
> "there's a program that tells you these — what's compatible and what's not" — @ggurov

### F7 as Primary epicEFI Platform

@mke asked for confirmation: "Ok yeah....but f7 is what epic is and supports yeah?"
@ggurov confirmed: "yes, f7 is on epic"

### Swapping MCUs for Bench Testing

@ggurov noted a practical use case for MCU swapability — being able to swap MCUs between ECUs during development and bench testing without needing separate hardware for each processor variant.
