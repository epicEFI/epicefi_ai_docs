# LiPo Battery Configuration for Automotive Use (Voltage and Balance Pins)

*Source: rusEFI Discord, 2025-11-27 | Channel: 1401895238481744052*
*Contributors: mynameisdeleted (@mynameisdeleted)*

## Summary

LiPo battery cell voltage limits and regulator configuration for using LiPo packs as automotive 12 V replacements. Slow-charging is safe at 4.0 V per cell; fast-charging requires 4.2 V per cell. A 3S pack is the minimum for 12 V use; a 4S pack allows a 16 V regulation point with balance taps at even intervals.

## Details

### Cell Voltage Thresholds

| Mode | Voltage per Cell |
|------|-----------------|
| Slow-charge (safe float) | 4.0 V |
| Fast-charge (full charge) | 4.2 V |

Balance circuits handle the milliamp-level current needed to equalize cells — the small differential (a few milliamps) is sufficient to drain the highest-charged cell back to the pack average.

### 3S Configuration (12 V nominal)

- **Cells in series:** 3
- **Regulation voltage:** 12.0 V
- **Balance tap positions:** 4.0 V and 8.0 V (taps at cells 1 and 2)

### 4S Configuration (16 V nominal)

- **Cells in series:** 4
- **Regulation voltage:** 16 V (set at regulator)
- **Balance tap positions:** 4 V, 8 V, and 12 V (one tap per inter-cell junction)

The 4S configuration provides a higher bus voltage which may benefit starter motor performance or be used in higher-voltage accessory systems.

### Charging / Regulator Notes

- An unregulated alternator (common on motorcycles, ATVs, and dirt bikes) can be used with a custom switching regulator. The duty-cycle modulation based on an ADC reference keeps cell voltage within bounds.
- A minimal regulator circuit requires: rectifier, coil, capacitor, and a switch driven by an Arduino 3.3 V digital output.
- The gate drive circuit for the main switching MOSFET can be built with a 1000 V N-channel MOSFET, a current-limiting resistor, and a PNP Darlington transistor that activates when base current is drawn.

### Context

This discussion occurred in the context of the microrusefi/epicEFI development channel focused on power supply design. The balance pin voltage values (4, 8, 12 V) correspond directly to per-cell tap points for passive or active cell balancing during charging.
