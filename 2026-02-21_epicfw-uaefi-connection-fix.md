# epicEFI Firmware: uaEFI Single-Connection Bug Fix Procedure

*Source: epicEFI Discord, 2026-02-21 | Channel: 1356732771325968630*
*Contributors: @Robb235, @ggurov*

## Summary

A bug was reported where a uaEFI board running epicEFI firmware would only allow TunerStudio to connect once. After disconnecting, TS could not reconnect until the ECU was power-cycled. The issue affects epicFW versions 2/17 and 2/20, and does not occur with the standard rusEFI firmware. The fix is a full flash procedure using a specific flashing sequence.

## Details

### Symptom

- TunerStudio connects to the uaEFI board running epicEFI firmware **one time only**.
- After disconnecting TS, **reconnection fails** — TS will not connect again.
- Power-cycling the ECU restores connectivity temporarily (one connection again).
- Confirmed affected firmware versions: **epicFW 2/17 and 2/20**.
- **Not affected**: boards running standard rusEFI firmware behave correctly.

### Fix: Full Erase + Flash with .hex File

`ggurov` provided the resolution:

1. **Flash the `.hex` file** (not a binary or other format).
2. Perform a **"Full Erase"** of the ECU before flashing.
3. The sequence is: Full Erase first, then flash the `.hex`.

This procedure is required when switching to epicFW or re-flashing epicFW — a partial flash without full erase appears to leave the firmware in a state that causes the reconnection bug.

### Notes

- This is a known firmware/flash procedure issue, not a hardware defect.
- Always use the `.hex` file format with full erase when flashing epicFW on uaEFI.
