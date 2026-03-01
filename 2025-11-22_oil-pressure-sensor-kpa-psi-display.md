# Oil Pressure Sensor Calibration: kPa Display Label vs PSI Input Values

*Source: rusEFI Discord, 2025-11-22 | Channel: 1411925674498719775*
*Contributors: fast335xi (@fast335xi), ToyotaTerrorist (@toyotaterrorist), Joesphan (@joesphan), Tera (@teraflopping), mynameisdeleted (@mynameisdeleted), oilpressureguru (@oilpressureguru)*

## Summary

The oil pressure sensor configuration in rusEFI/epicEFI has a hard-coded "kPa" unit label in the UI, but the actual calibration table values should be entered in PSI if you are using a PSI-range sensor. Entering values in kPa when the sensor output represents PSI will produce wrong readings. Additionally, dry-sump systems typically read 6–10 psi lower than wet-sump systems due to crankcase vacuum.

## Details

**Sensor calibration table — PSI values with kPa label:**
The sensor input calibration table maps sensor voltage to pressure. The UI displays "kPa" as the unit label, but this label is hard-coded and does not perform any unit conversion.

- If your sensor is a 0–145 PSI sensor (common 0.5–4.5V output type), enter:
  - **0 PSI at 0.5 V**
  - **145 PSI at 4.5 V**
- The displayed number will be the actual PSI value, despite the "kPa" label shown in the software.
- Entering kPa values (e.g., 0–1000) for a PSI sensor will cause the displayed reading to be wrong.

ToyotaTerrorist confirmed this caused confusion: entering values in kPa instead of PSI broke the readings. After correcting to PSI values, the display was accurate.

**Verifying readings — use the laptop, not the dash:**
When troubleshooting pressure readings, Joesphan advised checking values directly in the rusEFI/epicEFI software (tuning laptop) rather than relying on a third-party dashboard:

- Aftermarket dashes may apply their own unit conversions, introduce offset, or display cached/stale values.
- The laptop screen shows the raw ECU sensor value without any intermediate conversion, making it the ground truth for calibration verification.
- ToyotaTerrorist found the dash was actually reading correctly once the ECU calibration was fixed — the confusion arose from interpreting the dash reading before verifying the ECU side.

**Sanity check for running oil pressure:**
Tera noted: if the engine is running (not just cranking) and the display shows ~100, it is safe to interpret this as approximately 100 PSI, which is a reasonable oil pressure for a running engine.

**Dry-sump vs wet-sump oil pressure:**
mynameisdeleted and oilpressureguru discussed a common observation:

- Dry-sump systems typically show **6–10 PSI lower** oil pressure compared to equivalent wet-sump systems.
- The reason is crankcase vacuum: dry-sump systems evacuate the crankcase, which creates a slight vacuum that reduces the net oil pressure reading at the sensor.
- This is normal behavior, not a fault. Account for this offset when comparing logs or setting oil pressure warning thresholds between dry-sump and wet-sump configurations.
