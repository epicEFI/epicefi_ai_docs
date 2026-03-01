# Barometric Pressure Table Configuration to Avoid Fuelling Errors

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @jeybee*

## Summary

If the barometric pressure (baro) correction table is not configured correctly, the firmware will apply incorrect fuelling corrections. The safe default is to set all values in the baro table to 1 (unity/no correction) unless a proper baro reading source is available.

## Details

### The Problem

- The baro correction table applies a multiplier to fuelling based on ambient barometric pressure.
- If left at non-unity values without a real baro source, the firmware will **incorrectly adjust fuelling**, leading to rich or lean conditions that are hard to diagnose.

### Correct Configuration

Set **all values in the baro table to 1** unless one of the following baro sources is configured and working:

| Source | Notes |
|--------|-------|
| MAP sensor baro snapshot at startup | Firmware reads MAP at power-on (before cranking) as baro reference |
| Dedicated barometric pressure sensor | Separate sensor always reading ambient pressure |

### Practical Advice

- If you do not have a dedicated baro sensor and have not confirmed the firmware performs a MAP-based baro snapshot at startup, set the entire baro table to **1.0**.
- This effectively disables baro correction and prevents unintended fuelling changes due to incorrect baro data.
- Once a proper baro source is confirmed and calibrated, the table can be populated with real correction values.
