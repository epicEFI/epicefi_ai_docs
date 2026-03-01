# IACV Cold Start Issue and afterCrankingIACtaperDuration Fix

*Source: rusEFI Discord, 2025-11-18 | Channel: 1392172039430738111 (advanced-tuning)*
*Contributors: @Robb235, @ggurov*

## Summary

A user running E85 experienced the engine starting well but dying after about one second of idling at cold temperatures (30°F / 0°C). Log analysis revealed the Idle Air Control Valve (IACV) was closing too quickly after cranking. The fix involves editing `mainController.ini` to raise the `afterCrankingIACtaperDuration` limit from 500 to 65000 cycles, allowing a longer taper period before the idle valve closes down.

## Details

### Symptoms

- Engine fires and begins to idle normally after cold start on E85
- Engine dies after approximately 1 second of idling
- Issue is more pronounced at low ambient temperatures (~30°F / 0°C)
- Holding the throttle slightly open (adding TPS) during the initial idle prevents the stall

### E85 Cold Start Cranking Fuel

As context for the severity of cold-start conditions on E85: at 30°F (0°C), a 9x cranking fuel multiplier was needed to start the engine. Discussions on MS Extra forums have noted even higher multipliers (10x+) for E85 cold starts. Fuel condensation on cold intake surfaces is a major factor.

### Root Cause: IACV Closing Too Fast

Log analysis by @ggurov showed the idle position (IACV duty cycle / pink trace) closing too quickly while RPM was still trying to stabilize. On the second start attempt where the user held the throttle slightly open, the RPM stayed up long enough for the IACV to taper down successfully and maintain idle.

The relevant parameter is **`afterCrankingIACtaperDuration`**, which controls how many cycles the IACV takes to transition from the cranking open position to the normal idle position. The default configuration caps this value at 500 cycles, which may be too short for cold conditions.

### Fix: Increase afterCrankingIACtaperDuration Maximum

In `mainController.ini`, find the line:

```
afterCrankingIACtaperDuration = array, U16, 6242, [6], "cycles", 1, 0, 0, 500, 1
```

Change the maximum value from `500` to `65000`:

```
afterCrankingIACtaperDuration = array, U16, 6242, [6], "cycles", 1, 0, 0, 65000, 1
```

This is an INI-file change (TunerStudio configuration), not a firmware change. @ggurov noted this limit would be raised in a future firmware release, so the manual INI edit is a workaround until then.

### Persistence After Firmware Updates

INI file edits to `mainController.ini` are local TunerStudio configuration changes. After a firmware update, if the INI is regenerated, the change may need to be reapplied. However, routine firmware updates that do not change the INI definition for this parameter should not affect existing tune settings.
