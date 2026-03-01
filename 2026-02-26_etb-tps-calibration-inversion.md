# ETB TPS Calibration Inversion: Diagnosing and Fixing Inverted Throttle Direction

*Source: rusEFI Discord, 2026-02-25 | Channel: 1356732771325968630*
*Contributors: @Hydrocarbon, @Joesphan, @ggurov, @SanGawku*

## Summary

A user experienced persistent TPS signal inversion on an electronic throttle body: idle showed as WOT and full pedal showed as closed, regardless of which TPS was set as primary or how the closed/open calibration values were swapped in software. The correct diagnosis involved reading the ETB rest position voltage with "ETB disable" enabled, using those values to identify the actual open and close voltages, then swapping the ETB close and open configuration values in software. When software inversion was insufficient due to a firmware behavior, physically swapping the H-bridge motor output polarity (MOT+/MOT-) resolved the issue.

## Details

### Symptoms

- With **tps1 as primary**: autocalibration inverts the signal -- idle position reads as fully open (WOT), full pedal reads as closed.
- With **tps2 as primary, tps1 as secondary**: calibration completes but the system does not respond to pedal input at all.
- Manually swapping closed/open calibration values caused the throttle to go even further WOT at rest and jam shut at full pedal.
- Three ETBs tested, all showing the same behavior -- the issue is in configuration, not the throttle bodies.

### Concept: Reading ETB Rest Position to Determine Actual Open/Close Voltages

The correct approach for ETB calibration when the direction is inverted:

1. Enable **"ETB disable"** in the configuration. This cuts H-bridge control but allows TPS reading.
2. Observe the throttle blade's natural rest (spring-loaded) position voltage -- this is the voltage when the motor is not driven.
3. Run autocalibration to record both cal values (the min and max TPS voltages observed during the calibration sweep).
4. Compare the rest position to the cal values to infer which voltage corresponds to closed and which to open.

**Example** (@Joesphan):
- ETB rest position with "ETB disable" on: **1.2V**
- Autocalibration values: **0.8V** and **4.2V**
- Since the blade springs to ~1.2V (near closed), and 0.8V is lower than rest, **0.8V = closed** and **4.2V = open**
- If the system has these inverted (thinks 4.2V is closed), swap the ETB close and open values in the configuration.

### Software Inversion: Swapping ETB Close and Open Values

To correct a simple inversion without touching wiring:
- Navigate to the ETB calibration settings in TunerStudio/EpicTuner.
- Swap the values assigned to "ETB close" and "ETB open" positions.
- After making the change: **Burn**, then **disconnect USB**, then **power cycle the vehicle**. Changes may not take effect without a full power cycle.

No physical wire swapping is needed for a pure direction inversion -- this can be corrected entirely in software.

### When Software Inversion Is Not Sufficient

In @Hydrocarbon's case, swapping the calibration values in software did not fully resolve the issue. The throttle would open wider than expected at rest and jam shut at full pedal, suggesting the H-bridge output direction was the underlying problem.

**Resolution that worked**: Physically swapping the MOT+ and MOT- wires at the ETB connector (swapping H-bridge motor output polarity).

- @ggurov suggested: "flip no1 and no2 direction in h-bridge" and "try flipping no1 and no2 pins in h-bridge, then change which tps is tps1"
- After physically reversing the motor wires, the throttle behavior normalized.

This points to a firmware issue noted by @Joesphan: "Auto cal writes the same value in both [close and open]" -- suggesting a potential bug where autocalibration does not correctly differentiate close from open in certain conditions. The community noted this should be fixed and that an explicit invert mode should be added to avoid requiring physical wire swapping.

### Recommended ETB Calibration Procedure

1. With ETB disable ON, note the TPS voltage at rest (spring-loaded position).
2. Run autocalibration, write down the two resulting values.
3. Infer which value is close and which is open based on the rest position.
4. Enter correct close and open values manually if autocalibration assigns them incorrectly.
5. Burn settings, disconnect USB, power cycle the vehicle.
6. If the throttle is still inverted after software correction, physically swap MOT+ and MOT- wires and retry calibration.
7. Set TPS min valid and max valid values appropriately to avoid false limit conditions.

### Additional Notes

- TPS1 and TPS2 voltage should both sweep proportionally when the throttle blade is moved manually -- verify this before attempting calibration.
- The fact that TPS sweeps correctly but the system still inverts behavior can indicate H-bridge direction is wrong rather than a TPS wiring issue.
- Both rusEFI and epicEFI firmware share the same ETB calibration logic in this context.
