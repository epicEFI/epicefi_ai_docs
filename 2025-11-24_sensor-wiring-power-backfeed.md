# Sensor Wiring and Power Source — Avoiding ECU Backfeed

*Source: rusEFI Discord, 2025-11-24 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Jmoneus (@jmoneus), Tera (@teraflopping)*

## Summary

Explains the correct approach to powering sensors on rusEFI/epicEFI ECUs (including the E840), covering which sensors are powered internally by the ECU, which require external relay power, and why powering sensors from a separate supply can backfeed and keep the ECU powered after ignition off.

## Details

### Core Rule: Power Sensors from the Same Source as the ECU

All sensors that require an external supply voltage (12V) must be powered through the **same ignition relay that powers the ECU**. If a 12V-powered sensor is wired to a separate always-on supply (or even the battery directly through only a fuse), it can **backfeed the ECU through its sensor input**, keeping the ECU partially powered after the ignition is switched off.

The path that causes backfeed:
```
Battery → Fuse → Sensor (12V supply) → ECU sensor input → Ground
```
When the ECU is nominally off but its input pin is connected to a live 12V sensor, current can flow backward into the ECU's internal logic, keeping the microcontroller active.

**Correct wiring:** Sensor supply 12V must be switched by the same relay that powers the ECU, so both lose power simultaneously on ignition off.

### Sensor-by-Sensor Power Requirements

| Sensor Type | Power Source | Notes |
|---|---|---|
| MAP (manifold absolute pressure) | ECU-provided 5V | Safe; ECU powers and grounds it |
| IAT (intake air temperature) | ECU-provided | NTC/resistive; ECU provides excitation via pull-up |
| CLT (coolant temperature) | ECU-provided | NTC/resistive; same as IAT |
| TPS (throttle position sensor) | ECU-provided 5V | ECU powers and grounds it |
| Crank/cam sensor (Hall effect) | Ignition relay (same as ECU) | Hall sensors sink the pull-up voltage supplied by the ECU; they still need Vcc from a switched supply |
| Water/coolant temp (NTC) | ECU-provided | One terminal grounded, other goes to ECU; no external power needed |
| O2 sensor (narrowband/wideband) | Ignition relay (same as ECU) | Requires external 12V for heater circuit |
| Injectors / solenoids | Ignition relay (same as ECU) | 12V-powered actuators |

**Key distinction:**
- **MAP/IAT/CLT/TPS** — powered entirely from the ECU's internal regulators. These are safe regardless of sensor wiring because the ECU itself controls their supply.
- **Hall effect crank/cam sensors** — do not output a voltage themselves; they pull the ECU's pull-up line low when triggered. However, they still require their own Vcc supply. This supply **must** come from the same relay as the ECU. If it comes from an always-on source, the hall sensor's internal circuit can backfeed through the ECU's pull-up resistor.
- **Temperature (NTC) sensors** — purely resistive; they connect between the ECU input and ground. No external power rail; no backfeed risk.

### E840 ECU Specific Notes

The E840 ECU does not act as a relay for sensor power when the ECU itself is unpowered. Sensors that require 12V must have their supply switched externally (via ignition relay) and should not depend on the ECU to provide that switched 12V.

### Console-to-ECU Connection

The rusEFI console (TunerStudio) connects to the ECU over **USB**. There is no network or serial-only connection required for basic tuning.
