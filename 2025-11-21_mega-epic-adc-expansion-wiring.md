# Arduino Mega ADC Expansion (mega_epic) — Wiring and Signal Scaling

*Source: rusEFI Discord, 2025-11-21 | Channel: hardware-build (1411925674498719775)*

*Contributors: ggurov, GGGI (gunadi)*

## Summary

The mega_epic module (Arduino Mega running epicFW satellite firmware) provides additional analog inputs to the epicEFI system over CAN bus. Sensors are wired to the Mega's analog pins with a series input resistor (470 Ω or 1 kΩ recommended). The Mega sends raw ADC readings of 0–1024 to epicFW over CAN; the main ECU interprets this as a 0–5 V sensor signal and applies standard sensor calibration. Sensor calibration is done on the main ECU, not the Mega. This architecture isolates noise events to the Mega rather than the main ECU.

## Details

### Signal Chain

1. Sensor output → **470 Ω (or 1 kΩ) series input resistor** → Arduino Mega analog pin.
2. Arduino Mega reads the pin and sends the raw **0–1024 ADC value** over CAN to epicFW.
3. epicFW receives the value and maps it as **0–5 V** (i.e., ADC 0 = 0 V, ADC 1024 = 5 V).
4. Main ECU applies the configured sensor calibration table (same as a locally wired sensor).

- ggurov: *"mega sends 0-1024 to epicFW — that reads it as 0-5v, as if it was a local sensor"*

### Input Resistor

- **Recommended value: 470 Ω** (ggurov), or **1 kΩ** as an alternative.
- Purpose: protection against wiring faults, shorts, and transient events ("stupidity protection").
- An optional RC filter can be added if signal noise is a concern, but ggurov recommends skipping it for simplicity in most installations.
- Reference: similar input protection circuits used in Speeduino and UA4C boards.

### Power Supply

- The Mega's 5 V supply can power sensors connected to the ADC inputs.
- Grounds must be connected between the sensor and the Mega.
- Because the Mega connects to the main ECU over CAN (isolated), faults on the Mega's analog inputs do not propagate directly to the main ECU.
- ggurov: *"it's just less bad, cause it's over canbus, and isolated from the main ecu"*

### Sensor Calibration

- All sensor calibration (e.g., fuel level curve, oil pressure scaling) is configured on the **main ECU**, not the Mega.
- The Mega is transparent — it passes raw ADC counts; the ECU handles engineering unit conversion.

### Typical Use Cases

- Fuel level sensor
- Oil pressure sensor
- Other non-safety-critical auxiliary sensors

### Architecture Benefit

- Faults or damage to the Mega do not affect main ECU operation or engine management.
- Designed as an add-on for sensors that are desirable but not critical to engine running.
