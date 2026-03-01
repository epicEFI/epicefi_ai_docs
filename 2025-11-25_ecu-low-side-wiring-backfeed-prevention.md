# ECU Low-Side Component Wiring and Backfeed Prevention

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Joshan Lu (@joshseek_30236_07984), Jmoneus (@jmoneus)*

## Summary

Discussion covering which ECU-driven components are susceptible to backfeeding through the ECU outputs when unpowered, and the correct relay/circuit strategy to prevent it. Also explains why active silicon devices (Hall sensors) require external power to properly sink the ECU's pull-up voltage.

## Details

**Components susceptible to backfeed:**
Any component that the ECU drives on the low (ground) side, where the high (12V) side is supplied from a separate circuit, can backfeed current through the ECU output pin when the ECU is unpowered. Components in this category include:

- Fuel injectors
- Idle air control valve (idle valve)
- Fuel pump relay
- Main relay
- Any other relay or low-side switched load

**Wiring rule — low-side loads must share relay/circuit with the ECU:**
Everything driven low-side by the ECU — relays, injectors, idle valve — must receive its 12V supply from the same relay or circuit that powers down with the ECU at key-off. If a low-side load stays powered while the ECU is off, the 12V supply will find a path through the ECU's output driver back to ground, potentially keeping the load partially energized and stressing the ECU output stage.

> "Anything low-side (relay, injector, idle), anything that has 12v from the other side, needs to be on the same relay/circuit/etc, turning on/off with the main ECU." — ggurov

**Hall sensor power requirement (active silicon):**
Hall-effect sensors are active silicon devices — they require an external power supply (typically 12V from an ignition-switched relay) to operate. The ECU provides an internal pull-up voltage on the signal wire, but the Hall sensor sinks that pull-up to produce a logic-low signal. If the Hall sensor loses power (e.g., at key-off), it can no longer sink the pull-up, which can produce false signal transitions. Additionally, if the Hall sensor's supply rail is live while the ECU is off, the signal pin pull-up from the ECU is absent and the configuration is undefined.

> "They're active silicon, so they need power to do their job. If they power off, they can't sink the pullup the ECU provides." — ggurov

Practical consequence: Hall-effect cam and crank sensors should be powered from the same ignition-switched relay that powers the ECU, so the sensor powers down with the ECU. This also allows the engine to be stalled by switching the key off, since the crank/cam signals disappear simultaneously with ECU power.

**Two-relay EFI wiring architecture:**
A complete EFI system can be run on as few as two relays plus appropriate fusing:

1. **Ignition relay** — energized by the ignition key switch; powers the ECU and Hall-effect cam/crank sensors.
2. **Main power relay** — controlled by the ECU (latched on at startup); supplies 12V to injectors, idle valve, fuel pump relay, and other low-side loads.

This architecture ensures all low-side loads share a common switched 12V supply tied to ECU state, preventing backfeed paths.

**Chassis grounding note:**
Ground paths through the chassis should not be relied upon for carrying EFI signal return currents. The chassis is typically poorly grounded relative to the battery — there is no dedicated fat chassis rail for current return. Positive supply cables are generally adequate; ground straps are the weak point.

> "The positive cables are usually fine, it's the chassis that's poorly grounded. It's not like there's a nice fat chassis rail that can carry the ground." — Joshan Lu

All ground points must be sanded through paint to bare metal before bolting down. Measure resistance from the battery negative terminal to any engine ground — it should be only a few ohms. Account for test lead resistance when using extended leads.
