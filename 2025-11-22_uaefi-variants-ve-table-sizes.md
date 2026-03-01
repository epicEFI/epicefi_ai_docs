# uaEFI Variants and VE Table Sizes
*Source: rusEFI Discord, 2025-11-22 | Channel: 1356732771325968630*
*Contributors: @ggurov (ggurov), @fefco (fefco), @sheckta#1593 (sheckta_1593)*

## Summary

Clarifies the different uaEFI board variants, which firmware targets correspond to which hardware, and the VE (volumetric efficiency) fuel map table sizes available on each. The "BIGFUEL" suffix indicates a larger 32x32 VE table, configurable per-board in firmware.

## Details

### uaEFI Hardware Variants

There are (at minimum) two uaEFI hardware variants:
- **uaEFI** — standard variant
- **uaEFI_pro** — F7-based (STM32F7) uaEFI

The corresponding firmware targets are:
- **`uaefi`** — standard firmware build
- **`uaefiBIGFUELL`** — firmware build with enlarged fuel/VE table
- **`uaefi_pro`** — F7 variant

ggurov confirmed:
> "there's 2 versions of uaefi — uaefi, and uaefiBIGFUELL — uaefi_pro is f7 uaefi"

### VE Table Size: Standard vs. BIGFUEL

The standard uaEFI VE map has a default resolution. The `uaefiBIGFUELL` firmware build provides a **32x32** VE table (confirmed by fefco and ggurov).

However, ggurov noted that actual table sizes vary slightly depending on available memory:
> "some of them are 24x24 or so, depending on memory — but it's adjustable in firmware"

The table size is set through per-board configuration settings in the firmware source, not through a universal compile flag. ggurov noted Honda applications typically need more load/RPM range and resolution, which motivated enabling the larger table on certain builds:
> "hondas usually need more range/resolution, so figured why not"

### Using epicEFI Firmware on uaEFI Hardware

If you flash the epicEFI firmware onto a uaEFI board, it will run but will report standard VE table dimensions (not the larger BIGFUEL table), because the epicEFI firmware target uses different per-board memory layout settings than the uaEFI-specific build.

To get the 32x32 VE table on uaEFI hardware, use the **`uaefiBIGFUELL`** firmware target, not a generic epicEFI build.

### Checking Your Firmware Build

The firmware build target is embedded in the `.msq` tune file signature. If a tune file shows a `uaefi` signature but was opened with epicEFI TunerStudio INI, the table sizes will not match what the hardware actually supports under the correct firmware.
