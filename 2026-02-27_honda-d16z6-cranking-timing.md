# Honda D16Z6 Cranking Timing Issues with uaefi / rusEFI

*Source: rusEFI Discord, 2026-02-27 | Channel: 1356732771325968630*
*Contributors: @Hydrocarbon, @ggurov*

## Summary

A user running a Honda D16Z6 engine on uaefi/rusEFI could not get the engine to start reliably — it starts and runs fine on the stock P28 ECU, but the rusEFI configuration appears to fire with excessively advanced timing during cranking. The root causes identified were: (1) an incorrect trigger angle that drifts between ignition cycles when set with a timing light, and (2) a firmware warning (C6675) indicating the VVT cam sync position is too close to the primary crank trigger sync point. The recommended fix is to flip the crank edge or cam edge trigger setting and re-time.

## Details

### Symptom

- Engine cranks but will not start (or starts poorly) on uaefi with a D16Z6.
- Engine starts and runs correctly on the OEM P28 ECU using the same hardware.
- uaefi appears to be applying far too much ignition advance during cranking.

### Trigger Angle Drift Issue

- The user previously attempted to set the trigger angle using a timing light with spark plugs out.
- Observed behavior: each time the ignition was cycled, the timing light reading showed an additional ~20 degrees of advance — indicating the trigger angle was not stable or was being reset/corrupted between cycles.
- This suggests the trigger angle reference point is not being correctly locked, possibly due to the edge/polarity setting.

### Warning Code C6675: VVT Sync Position Too Close to Trigger Sync

- The firmware issued warning **C6675**.
- The firmware check that generates this warning:
  ```c
  if (absF(angleFromPrimarySyncPoint) < 7)
  ```
  This fires when the VVT cam sync event is within 7 crank degrees of the primary trigger sync point, which can cause ambiguity in sync detection.
- Engine configuration: **12/24** trigger wheel (12 teeth on crank, 24 on cam, or a 12-tooth pattern with cam reference — consistent with the D16Z6's distributor-based CAS system).
- The user was using the **included Honda OBD1 basemap** as a starting point.

### Recommended Fix

1. **Flip the crank trigger edge or cam trigger edge** in the trigger settings (rising vs. falling edge for the VR/Hall sensor).
2. **Re-time the engine** with a timing light after flipping the edge, with spark plugs removed to allow safe cranking.
3. Flipping the edge resolves the sync ambiguity that causes the C6675 warning and should stabilize the trigger angle between ignition cycles.

### Practical Notes

- Cold-weather conditions (working below 20°F / -7°C) were an additional constraint for the user; timing light verification requires outdoor work.
- The P28 ECU running correctly on the same engine confirms the hardware (sensors, wiring, trigger wheel) is good — the issue is purely in the rusEFI trigger configuration.
