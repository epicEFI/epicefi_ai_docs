# UAEFI Firmware Flashing and Wideband Sensor Programming via STLink

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @ggurov, @GGGI, @Chemex-joshseek*

## Summary

This document covers two related hardware bring-up tasks on a freshly built UAEFI: flashing the main ECU firmware and separately programming the wideband oxygen sensor. The main firmware is downloaded from the official epicEFI firmware hosting page and installed via the `flash` command. The wideband controller must be programmed independently using an STLink programmer. USB DFU mode requires direct USB drivers distinct from the normal serial chip drivers; a reboot of the host PC is sometimes needed to resolve driver issues.

## Details

### Installing Firmware on a Freshly Built UAEFI

A user building their first UAEFI asked how to get firmware onto it:

> "hey fellas, can I trouble you guys for a minute, how do I install the firmware on freshly build UAEFI?"

The recommended approach is to use the `flash` command in the terminal with the board connected. The latest firmware binaries are hosted at:

**https://content.epicefi.com/firmware/**

### Programming the Wideband Oxygen Sensor

The wideband oxygen sensor controller on epicEFI/rusEFI boards must be programmed separately from the main ECU firmware, and this requires an STLink programmer:

> "you have to program the wideband using stlink"

A user posted an image of their wideband sensor pinout and asked for confirmation. The pins were confirmed to match the standard wiring diagram for the wideband sensor used in rusEFI/epicEFI systems.

### USB DFU Mode and Driver Issues

When switching the UAEFI board between normal operation mode and DFU (Device Firmware Update) mode, different drivers are used:

> "so when its on normal it connects via the serial chip, but when in DFU its direct right, the drivers arent working but ill probably do a restart on the computer see if it fixes it"

- **Normal mode**: connects via the serial/UART chip (e.g., CH340 or similar)
- **DFU mode**: uses a direct USB connection with a separate WinUSB/DFU driver

If the DFU drivers are not loading correctly, restarting the host PC is a valid first troubleshooting step to force re-enumeration.
