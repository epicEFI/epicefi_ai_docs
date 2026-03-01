# 16x16 Main Table Limitation Fixed in epicEFI

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @mke, @ToyotaTerrorist*

## Summary

The 16x16 main fuel/ignition table size limitation that was present in stock rusEFI has been resolved in epicEFI. This was a known pain point for tuners who needed larger lookup tables for high-resolution tuning. The fix is implemented in the epicEFI fork; the current epicEFI firmware supports 32x32 fuel tables and 20x20 ignition tables while still fitting in the available RAM on STM32F4 hardware.

## Details

### The Problem

Stock rusEFI historically had 16x16 as the maximum main table size. @mke referenced this as a significant barrier:

> "And bs 16x16 main tables..." — @mke

This was cited as one of the factors that kept some users from adopting rusEFI, alongside limited I/O counts on certain hardware.

### The Fix in epicEFI

@ToyotaTerrorist asked: "I'm pretty sure that got fixed no?"

@mke confirmed: "Yea, on epic" (with a winking emoji indicating satisfaction with the epicEFI solution).

### Current Table Sizes Confirmed

@ggurov confirmed that even on the lower-RAM STM32F4, the firmware supports:
- **32x32 fuel table**
- **20x20 ignition table**
- All other standard features simultaneously

> "32x32 fuel 20x20 ign fits — with all the other stuff" — @ggurov

This resolves the tuning resolution limitation for high-performance applications.
