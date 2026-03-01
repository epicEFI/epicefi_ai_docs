# Sensor Wiring: IAT, Coolant, and TPS on the 22-Pin Connector

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: DCwerx (@dcwerx)*

## Summary

DCwerx documented the pin assignments for wiring common engine sensors (IAT, coolant temperature, and throttle position sensor) to the 22-pin ECU connector. All sensors share pin 18 for sensor ground. The 5V reference is on pin 8. The TPS signal typically goes to pin 4.

## Details

**IAT (Intake Air Temperature) Sensor — 2-wire:**
- Pin 18: Sensor ground
- Respective input pin: Signal wire

**Coolant Temperature Sensor — 2-wire:**
- Pin 18: Sensor ground
- Respective input pin: Signal wire

Both IAT and coolant sensors are 2-pin sensors (ground + signal only). They do not require a 5V reference supply from the ECU — they are pull-up resistor-based NTC thermistors that only need a signal wire and a ground return.

**TPS (Throttle Position Sensor) — 3-wire:**
- Pin 18: Sensor ground
- Pin 8: 5V reference supply (22-pin connector)
- Pin 4: Signal (typical assignment on the 22-pin connector)

The TPS requires three connections because it is a potentiometer-type sensor that needs an excitation voltage (5V), a ground return, and a variable voltage signal output proportional to throttle opening.

**Key point:** All sensor grounds for this connector route to pin 18 — this is the shared sensor ground reference and must not be confused with chassis or power ground.
