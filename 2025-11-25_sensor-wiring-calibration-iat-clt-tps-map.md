# ECU Sensor Wiring and Calibration: IAT, CLT, TPS, MAP, and Wideband O2

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Jmoneus (@jmoneus), DCwerx (@dcwerx)*

## Summary

Wiring requirements and calibration needs for the four essential ECU input sensors (IAT, CLT, TPS, MAP), plus wideband O2 sensor integration. Covers internal pull-up behaviour for NTC sensors, 5V reference supply usage, sensor ground routing, and the E840/UA4C (Speeduino-compatible) wiring context.

## Details

**Sensor wiring — pin assignments by sensor type:**

| Sensor | Pin 1 | Pin 2 | Pin 3 |
|--------|-------|-------|-------|
| IAT (Intake Air Temp) | Sensor ground | ECU IAT input | — |
| CLT (Coolant Temp) | Sensor ground | ECU CLT input | — |
| TPS (Throttle Position) | Sensor ground | ECU 5V reference | ECU TPS input |
| MAP (Manifold Absolute Pressure) | Sensor ground | ECU 5V reference | ECU MAP input |

**IAT and CLT — NTC resistive sensors, internal pull-up:**
IAT and CLT sensors are NTC (negative temperature coefficient) thermistors. They require no external power supply. The ECU provides an internal pull-up resistor (typically 4.7 kΩ or 10 kΩ) tied to an internal 5V (or 3.3V on some platforms) reference. The sensor forms a voltage divider with the pull-up, and the ECU reads the resulting voltage to calculate temperature.

Correct wiring: one sensor terminal to sensor ground at the ECU, other terminal to the ECU's dedicated IAT or CLT input pin. Do not connect an external 5V supply to these sensors.

> "For IAT/CLT, one end of sensor goes to ground, another goes to ECU input."
> "The pullup to 5V is internal for CLT/IAT." — ggurov

On rusEFI/epicEFI (UA4C platform): internal pullup is nominally 4.7 kΩ to 3.3V (equivalent to the standard Speeduino 10 kΩ to 5V divider for sensor curve compatibility purposes).

**TPS and MAP — require ECU 5V reference:**
TPS and MAP sensors are ratiometric — they need a stable 5V excitation from the ECU to produce a proportional output voltage. Connect the sensor's Vref pin to the ECU's 5V sensor supply output, sensor ground to ECU sensor ground, and signal to the ECU analog input.

> "TPS is ground, 5V, TPS input."
> "MAP is ground, 5V, MAP input." — ggurov

**Sensor ground routing:**
All sensor grounds (IAT, CLT, TPS, MAP signal returns) must route back to the ECU's dedicated sensor ground pin(s), not to chassis ground or engine block ground. Sharing sensor ground with chassis introduces noise on the 5V reference and signal lines, degrading accuracy.

> "You would need all these sensor grounds to go back to the ECU, not external." — DCwerx

**Calibration requirements:**
CLT, IAT, TPS, and MAP must be calibrated in the ECU software to produce readings close to actual physical values before tuning. The ECU uses these for all fuelling and ignition calculations:

- **CLT/IAT**: Select the correct sensor curve or characteristic in the ECU (thermistor lookup table). Common aftermarket sensors use Bosch or GM curves. Verify that indicated temperature matches a known reference (e.g., ambient air temp for IAT at cold start).
- **TPS**: Calibrate the closed-throttle and wide-open-throttle voltage endpoints. The ECU reads throttle position as a percentage derived from this range.
- **MAP**: Verify that indicated manifold pressure matches atmospheric at key-on with engine off (should read close to local baro pressure, ~100 kPa at sea level).
- **Wideband O2**: Calibrate the output curve. Most external wideband controllers (e.g., LC-2, Innovate) output 0–5V linearly mapped to an AFR or lambda range. A common mapping is 10–20 AFR = 0–5V (configurable in the ECU's O2 calibration table). Custom curves are supported.

> "CLT/IAT/TPS/MAP need to read close enough to reality."
> "O2 needs to read right for fuel feedback."
> "Those [wideband controllers] are simple, usually 10–20 AFR = 0–5V. Custom is doable also." — ggurov

**O2 sensor — wideband required for closed-loop fuel control:**
A narrowband (stock) O2 sensor is not well-suited for closed-loop fuelling on a standalone ECU. Its output is a simple rich/lean toggle (step function at stoichiometric), not a proportional lambda reading. A wideband O2 controller with a 0–5V analog output is required for reliable fuel feedback and tuning.

**E840 / UA4C wiring reference:**
The DCwerx E840 ECU is based on the UA4C hardware, which is a Speeduino-derived platform (H7-based). All sensor wiring follows standard Speeduino conventions. The Speeduino wiring reference at https://wiki.speeduino.com/en/wiring/system is directly applicable. Speeduino, MegaSquirt, and most other aftermarket ECUs share the same sensor wiring conventions because automotive sensor standards are broadly consistent across platforms.

> "Pretend you're wiring a Speeduino, cause you are basically wiring a Speeduino. That E840 is a UA4C ECU, for Speeduino." — ggurov
