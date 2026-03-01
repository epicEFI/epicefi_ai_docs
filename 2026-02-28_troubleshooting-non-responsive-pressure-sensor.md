# Troubleshooting a Non-Responsive Pressure Sensor

*Source: epicEFI Discord, 2026-02-28 | Channel: 1356732771325968630*
*Contributors: @Bearded_Bogan, @Joesphan*

## Summary

When a pressure sensor (specifically a Bosch/Honeywell-style analog sensor) shows no output at the ECU despite confirmed wiring, the fastest diagnostic path is to remove the sensor, power it on the bench, and apply pressure by mouth while measuring the output voltage with a DMM. This isolates whether the fault is in the sensor itself, the wiring, or the ECU input.

## Details

### Symptoms

@Bearded_Bogan was troubleshooting an oil pressure sensor that registered nothing at the ECU despite all other sensors (CLT, IAT, TPS, PPS) functioning correctly. The wiring had been checked multiple times. Removing the sensor briefly caused the engine to shed a small amount of oil, confirming actual oil pressure existed — so the engine was not the problem.

### Wideband Power Supply Note

During the same session, a question arose about whether there was resistance between the 5V supply pin and the sensor ground at the ECU connector. @Joesphan clarified the key rule: **a wideband oxygen sensor must not be connected to the ECU's 5V reference supply.** Widebands operate from 12V (or their own dedicated controller supply) and have their own ground reference. Connecting a wideband to a 5V sensor input will not work and risks damage. This note came up because the user initially confused the wideband circuit with the 5V sensor supply continuity check.

### Bench Test Procedure for Pressure Sensors

To verify a pressure sensor independently of the vehicle wiring and ECU:

1. **Remove the sensor** from the engine or manifold.
2. **Power the sensor on the bench** using the correct supply voltage (typically 5V for analog pressure sensors; 12V for some Bosch sensors — verify with the datasheet).
3. **Connect a DMM to the signal output pin**, measuring DC voltage relative to sensor ground.
4. **Apply pressure** by fitting a short tube to the sensor port and blowing with your mouth, or using a hand pump. For vacuum sensors, simply covering the port or drawing a vacuum is sufficient.
5. **Observe voltage change.** A functional sensor will show a measurable voltage shift as pressure changes. A dead sensor will show no response or a fixed voltage.

If the bench test shows a live signal, the fault is in the wiring or ECU input. If there is no response on the bench, the sensor is faulty and should be swapped.

### Multimeter Requirement

@Joesphan emphasized that a working multimeter is a prerequisite for this kind of diagnosis. Without the ability to measure voltage and resistance reliably, fault-finding on analog sensor circuits becomes guesswork. A basic DMM adequate for automotive diagnostics is available for as little as $5 USD.

## Notes

- In this specific case, swapping to a spare sensor was the next planned step, which is a valid shortcut if a known-good sensor is available.
- The bench test procedure applies equally to MAP, oil pressure, fuel pressure, and boost pressure sensors.
- Always confirm the sensor's supply voltage requirement before bench testing — applying 5V to a 12V sensor or vice versa can damage it.
