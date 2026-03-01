# Migrating Tunes Between FOME and epicEFI Firmware
*Source: rusEFI Discord, 2025-11-22 | Channel: 1356732771325968630*
*Contributors: @Joesphan (joesphan), @Tera (teraflopping), @MykkH (mykkh)*

## Summary

FOME and epicEFI are separate firmware forks sharing common rusEFI roots, but their tune files are not directly interchangeable. A tune (.msq) from a FOME build cannot be flashed directly onto an epicEFI build without reconfiguration. This document describes the correct migration procedure and common failure modes.

## Details

### Tunes Are Not 1:1 Compatible

Joesphan stated explicitly:
> "the firmwares are not 1:1, you can't flash one tune directly over — have to reconfigure"

This is because FOME and epicEFI may differ in:
- INI field layout and field names
- Default values for unconfigured tables
- Trigger pattern dropdown entries (epicEFI may have different named patterns than FOME)
- Output pin assignments
- Feature flags and extension settings

### Correct Migration Procedure

When moving from a FOME tune to epicEFI (or vice versa):

1. Flash the **target firmware** (epicEFI or FOME) onto the ECU
2. Open TunerStudio using the **matching INI file** for that firmware build (not the old firmware's INI)
3. Start from a **fresh/default tune** — do not attempt to load the old .msq directly
4. **Manually re-enter all settings** from the old tune — Tera confirmed doing this:
   > "yes I know, that's what I did when I downloaded epic — I remade it by hand"
5. After entering settings, go through **every table** to verify no values are null, missing, or inaccurate (MykkH recommendation)
6. Specifically verify all **input pin assignments** — cam trigger inputs were found missing in an epicEFI tune that had been migrated from FOME

### Common Issues Found in Migrated Tunes

From the N63 bring-up session, issues found in the epicEFI tune after manual migration from FOME:
- **Cam trigger inputs missing** — input pins were not assigned in the epicEFI tune
- **Cam trigger lobe count set to 0** — the HPFP cam config had 3 lobes in FOME but was 0 in epicEFI
- **Injection disabled** — was intentional in this case (to return fuel control to stock DME), but easy to miss if migrating for actual fueling
- **Tach output not assigned** — in this setup the tach is handled by stock DME over CAN, so this was not an error, but worth checking on other builds

### Verifying Trigger Pattern Availability

Trigger patterns available in FOME may or may not have the same name or be available in epicEFI's dropdown. For the N63 case, the custom N63 trigger was added to epicEFI by ggurov and is available in the dropdown. Verify the correct pattern name before assuming it transferred.

### epicEFI INI Files

All epicEFI release INI files are available at:
`https://content.epicefi.com/firmware/ini/master/2025/11/`

Use the INI that matches the firmware version flashed to the ECU.
