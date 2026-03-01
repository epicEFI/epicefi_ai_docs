# MOSFET Selection and Gate Driver Considerations for rusEFI Adapter Boards

*Source: rusEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @hydrocarbon82, @sangawku, @ggurov, @robb235_00916*

## Summary

Hydrocarbon documented research into MOSFET and gate driver selection for custom adapter boards targeting the uaefi platform, which lacks robust high-current output stages. The discussion compared the VNLD5090 (an existing choice) against a newly found low-Rds(on) 3.3V-compatible MOSFET, and evaluated the cost/benefit of adding a dedicated dual-output gate driver. A parallel thread covered an ESP32-based design for 8 MOSFET outputs with ADC voltage dividers, including a tip on using global labels in KiCad to manage schematic complexity.

## Details

### MOSFET Comparison

| Part | Rds(on) | Gate Voltage | Notes |
|------|---------|-------------|-------|
| VNLD5090 | 90 mOhm | — | Existing low-power option used in some designs |
| New candidate | 16 mOhm | 3.3V compatible | Tiny gate charge; 5-6x lower resistance |

- The 16 mOhm MOSFET costs approximately $0.36 per unit
- Lower Rds(on) significantly reduces heat dissipation at higher currents
- Both are considered for "low power" applications in the engine management context

### Gate Driver Recommendation

- Dual-output gate driver: ~$0.36, capable of driving 20A MOSFET gates from a 3.3V input
- Requires only a 5-10 ohm gate resistor — no additional boost circuitry needed
- SanGawku advised against using the additional driver where not strictly necessary ("drop the driver")
- Design goal for adapter boards: at least 4 robust MOSFET outputs (uaefi only provides weak output stages by default)

### ESP32-Based MOSFET Output Board

Hydrocarbon is designing an adapter board using an ESP32 microcontroller:
- Controls 8 MOSFETs
- 8x ADC voltage dividers for analog sensor inputs
- Schematic noted as visually complex ("looks a little busy")

**ESP32-S2 ADC Characterization (tested):**
- ADC is accurate and consistent ("dead-on")
- Actual input range: **0 to 2.6V** (not 0 to 3.3V as commonly assumed)
- For reading 5V sensors: use a 2:1 voltage divider, which maps 5V to 2.5V — within the 2.6V range
- ESP32-S3 ADC was noted as inferior ("just the S3 is shit")

### Schematic Organization Tips (KiCad)

When schematics become complex with many connections (e.g., 8 MOSFETs + 8 ADC dividers + an ESP32):
- **Global labels** (net flags): Recommended approach to avoid wire "spaghetti". Connect net labels instead of drawing wires across the schematic.
- **Buses**: Also available in KiCad but require more time investment
- **Net flags**: SanGawku uses net flags (similar to global labels) for the same purpose
- Note: KiCad is described as click-intensive; shortcuts help but global labels are the most efficient approach for dense schematics
