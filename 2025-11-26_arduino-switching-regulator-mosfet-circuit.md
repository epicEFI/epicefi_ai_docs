# Arduino-Driven Switching Buck Converter Circuit for Motorcycle Battery Systems

*Source: rusEFI Discord, 2025-11-26 | Channel: 1401895238481744052*
*Contributors: mynameisdeleted (@mynameisdeleted)*

## Summary

Design notes for a switching buck converter circuit controlled by an Arduino digital output pin, intended for use with unregulated motorcycle/ATV alternators and LiPo battery packs. The circuit uses a high-voltage N-channel MOSFET switched via a PNP Darlington transistor, with duty cycle modulated from an ADC reference.

## Details

**Application context:**
Motorcycles, ATVs, and dirt bikes typically use unregulated alternators. The OEM regulator uses a bridge rectifier feeding a PWM stage that modulates duty cycle based on observed voltage (ADC reference). This circuit adapts that concept for an Arduino-controlled switching supply.

**Core circuit topology:**
A buck (step-down) switching converter with the following components:

| Component | Spec / Role |
|-----------|-------------|
| N-channel MOSFET | 1000V rating; main switching element |
| Resistor | Current limiting on gate drive |
| PNP Darlington transistor | Level-shifts Arduino 3.3V/5V digital output to drive MOSFET gate; turns on when current drains from the base |
| Rectifier diode | Bridge rectifier on alternator input |
| Inductor (coil) | Energy storage element in buck stage |
| Capacitor | Output filtering |

**Control:**
The Arduino digital output (3V logic) drives the PNP Darlington base. The Darlington switches the MOSFET gate at the desired frequency and duty cycle. Duty cycle is modulated based on an ADC-measured voltage reference to regulate output.

**LiPo battery target voltages:**
The circuit was designed around a LiPo battery pack:

| Configuration | Target voltage | Balance pin targets |
|--------------|----------------|-------------------|
| 3S LiPo | 12.0V regulated | 4.0V and 8.0V |
| 4S LiPo | 16.0V regulated | 4.0V, 8.0V, 12.0V |

- Fast charge max: 4.2V per cell
- Slow/maintenance charge: 4.0V per cell (works reliably)
- Balance maintained at a few milliamps delta to bleed down the highest-charged cell

**Note on MOSFET voltage rating:**
The 1000V rating is significantly higher than needed for a 12V or 16V system; this provides a large margin against voltage spikes from the unregulated alternator source.
