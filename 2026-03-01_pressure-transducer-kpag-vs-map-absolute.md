# Pressure Transducer Output: kPaG (Gauge) vs MAP (Absolute)

*Source: epicEFI Discord, 2026-03-01 | Channel: 1356732771325968630*
*Contributors: @MykkH, @Bearded_Bogan, @Joesphan, @ggurov*

## Summary

When selecting a pressure sensor for MAP input, it is important to distinguish between absolute-pressure sensors (which output MAP directly) and gauge-pressure transducers (which output kPaG — pressure relative to atmospheric). Dataq-style industrial pressure transducers commonly output kPaG, not absolute pressure, and require a calibration adjustment or offset in the ECU if used on a MAP input.

## Details

The conversation arose when a high-range pressure sensor was being evaluated as a replacement MAP sensor. The sensor in question reads from 0 kPa (full vacuum) to approximately 600 kPa absolute (≈86 psi boost) with a 0.5 V–4.5 V output range, and reads 1.1 V at atmospheric pressure.

The key distinction raised:

- **Absolute pressure sensors (MAP sensors)**: output voltage represents total pressure above absolute zero. At sea level atmosphere the output is ~100 kPa absolute. This is what the rusEFI/epicEFI ECU expects on its MAP input.
- **Gauge pressure transducers (kPaG)**: output voltage represents pressure above the current atmospheric reference. At atmospheric pressure the output is 0 kPaG (minimum voltage). Common in industrial instrumentation, including Dataq-brand transducers.

If a gauge-pressure transducer is wired to a MAP input calibrated for an absolute sensor, the ECU will read approximately 0 kPa at idle (instead of ~100 kPa), causing the load calculation to be severely wrong. Boost readings will also be offset by ~1 bar.

### Identification

- Read the datasheet: look for "gauge" (kPaG, psig) vs "absolute" (kPaA, psia, MAP).
- At atmospheric pressure with the sensor powered: an absolute sensor should output approximately its atmospheric reading (e.g. ~1.1 V for a 0–600 kPa / 0.5–4.5 V sensor); a gauge sensor will output its minimum voltage (≈0.5 V).
- Verify with a multimeter before wiring to the ECU input.

### Calibration in rusEFI/epicEFI

If only a gauge transducer is available, it can be used by applying a fixed offset equal to local barometric pressure in the sensor calibration, or by using a custom calibration curve. However, this approach loses the ability to track barometric correction automatically. An absolute sensor is strongly preferred for MAP.

## Notes

- High-range sensors (e.g. 0–600 kPa / 0–86 psi) trade off vacuum resolution — at idle, all the meaningful signal sits in the bottom ~15% of the ADC range. For naturally aspirated or mildly turbocharged engines, a 0–200 kPa or 0–300 kPa absolute sensor gives better resolution.
- The sensor discussed (1.1 V at atmosphere, 4.5 V at 600 kPa absolute, 0.5 V at full vacuum) was confirmed to be an absolute-type sensor suitable for MAP use, despite its industrial high-pressure range.
