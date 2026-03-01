# CAN-Bus Combo Sensors: Humidity, Pressure, and Temperature

*Source: rusEFI Discord, 2025-11-21 | Channel: hardware-build (1411925674498719775)*

*Contributors: fast335xi, Joesphan, ggurov*

## Summary

Community members identified two Amphenol Sensors CAN-bus combo sensors capable of measuring humidity, barometric pressure, and temperature in a single unit. These are of interest for more accurate charge air density calculations in engine management. The question of how humidity affects air mass calculations was raised but not conclusively resolved in this session. Both identified sensors communicate over CAN bus, making them compatible with the epicEFI CAN expansion architecture.

## Details

### Identified Sensors

#### Amphenol 3389 Combination Sensor
- Measures: **Humidity, Pressure, Temperature**
- Interface: **CAN bus**
- Product page: [https://amphenol-sensors.com/en/thermometrics/assemblies/3389-combination-sensor](https://amphenol-sensors.com/en/thermometrics/assemblies/3389-combination-sensor)

#### Amphenol A-2075 QUAD-CAN
- Measures: **Temperature, Humidity, Barometric Pressure, Intake Pressure** (four channels in one unit)
- Interface: **CAN bus**
- Notable: includes both barometric and intake pressure in the same sensor — potentially useful for combined ambient + manifold pressure sensing.

### Why Humidity Matters for Air Mass

- Humidity affects the density of the intake charge: moist air contains water vapor which displaces oxygen molecules, reducing effective air mass per unit volume.
- This was raised as an open question: *"did we ever come to consensus on how humidity affects air amount?"* — no definitive implementation answer was given in this session.
- Incorporating humidity data would allow the ECU to correct the calculated air mass for ambient moisture content, improving fuel delivery accuracy in highly variable climates.

### Integration Notes

- Both sensors use CAN bus communication — compatible with the epicEFI CAN expansion (mega_epic or direct CAN).
- CAN ID and protocol specifics would need to be mapped in firmware for actual use.
- The A-2075 QUAD-CAN is particularly attractive as it provides barometric reference alongside intake pressure and ambient temperature/humidity in a single device.
