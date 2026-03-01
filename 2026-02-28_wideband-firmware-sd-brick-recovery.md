# Wideband Controller Bricked by SD Firmware Flash — Recovery via ST-LINK

*Source: rusEFI Discord, 2026-02-28 | Channel: 1401895238481744052*
*Contributors: @ggurov, @tmbryhn*

## Summary

Flashing wideband (WB) controller firmware via SD card or via the TunerStudio "2025" update path currently bricks the WB MCU, leaving it unresponsive after a power cycle. Recovery requires a full chip erase and re-flash using an ST-LINK programmer. The ECU firmware is unaffected — only the WB MCU is corrupted.

## Details

### Failure Modes

Two confirmed paths that brick the WB controller:

1. **SD card flash**: Place the firmware file on the SD card in the correct directory. The filename must not display a `.bin` extension (i.e., file extensions must be visible in the OS so the file appears without the double extension). The WB Wideband Tools diagnostic reports "Busy" after the update starts, but the process never completes. After a power cycle, the WB controller is non-responsive.

2. **TunerStudio "2025" update path**: Using the "2025" firmware update option in TunerStudio (without an SD card) produces the same result — WB MCU is corrupted and stops connecting.

### Scope of Damage

- Only the **WB MCU** is affected.
- The **ECU firmware** is unaffected and continues to operate normally.
- The WB controller simply stops connecting/communicating post-update.

### Firmware File Path

The correct firmware file for the wideband controller is:

```
fw/wbo/wideband_image_with_bl.bin
```

### Recovery Procedure

1. Connect an **ST-LINK** programmer to the WB controller's SWD header.
2. Perform a **full chip erase** of the WB MCU.
3. **Re-flash** the correct firmware binary (`wideband_image_with_bl.bin`) using ST-LINK.
4. Power cycle the controller — WB function is restored.

## Notes

- This is a known issue as of 2026-02-28; the SD/TS update paths for WB firmware are effectively non-functional and dangerous to use.
- Until a fix is released, avoid using the SD card update or TunerStudio "2025" WB update path.
- An ST-LINK (or compatible SWD programmer) is required for recovery — there is currently no software-only recovery path.
