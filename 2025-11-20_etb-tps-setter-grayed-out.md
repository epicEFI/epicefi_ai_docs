# ETB/TPS Auto-Calibration Setter Grayed Out in TunerStudio

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov, Tera (teraflopping)*

## Summary

The ETB (Electronic Throttle Body) auto-calibration button and TPS setter buttons in TunerStudio are intentionally gated behind a 12 V ignition signal (Vign). They will appear grayed out or inactive if the ECU is powered from USB alone (bench power without switched 12 V). Once 12 V ignition is applied, the buttons become active.

## Details

- The **ETB1 auto-calibrate** function requires 12 V on the ignition input (`Vign`) to become active. This is a safety requirement — the ETB motor must not be driven without a valid ignition-on state.
- The condition is enforced in the epicEFI firmware; attempting to calibrate without 12 V will leave the button non-functional.
- Tera confirmed the buttons came live after applying 12 V, resolving the issue.

### Duplicate Menu Paths

- The Lambda/O2 sensor settings appear in **two places** in TunerStudio: under the Sensors tab and under the CAN tab. Both paths point to the same underlying setting — they are not separate configurations. Changing one changes the other.
- This was a source of confusion when trying to enable/disable CAN UEGO vs. analog O2.

### ETB Calibration Bug (Voltage Units)

- After running the auto-calibrate wizard, TPS values may appear incorrect ("really odd values").
- Root cause: the calibration values were converted to volts by andrey, so manual entry using raw voltage values (rather than ADC counts) is required.
- **Workaround**: Enter TPS calibration values by hand using voltage units rather than relying on the auto-calibration wizard result.
