# TPS Auto-Calibration Failure on Dual ETB: Workaround

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: Tera (@teraflopping), Joesphan (@joesphan), fast335xi (@fast335xi), ggurov (@ggurov)*

## Summary

The TPS auto-calibration routine in epicEFI/rusEFI can fail on dual electronic throttle body setups, leaving TPS2 with both open and closed calibration values set to the same number (e.g., both at 0 or both at the last read ADC value). This causes the ETB control loop to malfunction. The workaround is to manually set TPS2 closed to a sane value (e.g., 50), perform a full power cycle, and then re-run calibration.

## Details

### Symptom

After running the auto-calibration wizard:
- TPS1 calibrates normally (open and closed show different values).
- TPS2 open and closed values are identical — the ADC reading does not differentiate between the two positions.
- The ETB 1 (or ETB 2) does not move in response to throttle commands.
- PPS (pedal position sensor) continues to work correctly.

In the observed case: TPS2 ADC was moving (to approximately 66%) but the open/closed calibration fields were resetting or being overwritten with the same value, making the ETB think TPS2 is always at a fixed position.

### Root Cause

The auto-calibration routine has a known bug in some epicEFI firmware versions where TPS2 calibration entries are reset or incorrectly populated, particularly on dual-TB configurations. Joesphan noted this was also a problem in earlier rusEFI firmware (pre-epicEFI) and was believed to have been fixed in epicEFI, but it has resurfaced.

The firmware change introduced by an AI coding assistant ("Cursor") in this session may have also contributed, but the bug exists independently of those changes.

### Workaround

1. Manually set **TPS2 closed** to a reasonable value (e.g., **50**) directly in the settings, without using the auto-calibration wizard.
2. Also set **TPS4 closed** (second sensor on the second throttle body) to a normal value if applicable.
3. Perform a **full power cycle** (not just a software reboot — disconnect and reconnect power).
4. After power-up, attempt auto-calibration again, or manually enter the open/closed ADC counts by reading the live gauge values and typing them directly into the calibration fields.

fast335xi: "To me, jot down the number and might have to enter manually if it's failing or inversed."

### Rolling Back Firmware

If the workaround does not resolve the issue, rolling back to a previous known-good firmware version restored normal ETB behavior in this session. After rollback, fuel delivery also resumed functioning (which had broken in the newer firmware), suggesting the newer build had multiple regressions.

### Related Note: Burn Crashes TunerStudio

In the same session, clicking "Burn" in TunerStudio was crashing (breaking the connection or causing a firmware fault) before the TPS calibration issue was resolved. This appeared to be a downstream effect of the corrupted TPS2 calibration state, not a standalone burn bug. After reverting firmware and re-calibrating TPS, Burn worked normally.
