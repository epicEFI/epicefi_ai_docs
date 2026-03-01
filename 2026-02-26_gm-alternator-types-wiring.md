# GM Alternator Types and Wiring Requirements

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @Hydrocarbon*

## Summary

GM alternators span three distinct generations with significantly different wiring requirements. The old single-wire type is the simplest, the Gen3 LS alternator needs only a resistor or bulb on the exciter wire, and the Gen4 LS alternator requires a PWM signal from the ECU. High-amperage compact relays (105–160A) using a 4/2-pin configuration are available for the high-current switching needs these alternators may involve.

## Details

### GM Alternator Generations

| Generation | Connector | Wiring Requirement |
|---|---|---|
| Old-school | 1-wire | Self-exciting; minimal wiring needed |
| Gen3 LS-V8 | 4-pin | Requires a bulb or **470 ohm resistor** on the exciter circuit |
| Gen4 LS | 2-pin | Requires a **128 Hz PWM signal** from the ECU |

#### Old-School 1-Wire Alternator
- Simplest to wire
- Self-exciting once engine is running
- Only requires the main charge wire to the battery/bus bar

#### Gen3 LS-V8 Alternator (4-Pin Connector)
- Found on Gen3 LS V8 engines
- The exciter/enable pin requires either:
  - A **small bulb** (traditional charge warning lamp circuit), or
  - A **470 ohm resistor** to switched 12V
- No PWM control needed; output voltage is fixed

#### Gen4 LS Alternator (2-Pin Connector)
- Found on Gen4 LS engines
- Requires a **128 Hz PWM signal** on the field/control pin
- The duty cycle of the PWM signal controls alternator output voltage
- Must be driven from the ECU or a dedicated alternator control circuit
- Suitable for rusEFI PWM output control via Lua or built-in alternator control

### Compact High-Current Relays

For switching high-current loads (fans, fuel pumps, etc.) in tight spaces:
- Available in **4-pin and 2-pin** configurations
- Current ratings: **105A to 160A**
- Described as "fairly compact" for their current rating
- Suitable for main power switching where a traditional large relay box is not desired
