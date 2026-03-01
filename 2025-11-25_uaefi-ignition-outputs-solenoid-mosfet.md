# Using uaEFI Ignition Outputs for Transmission Solenoid Control via External MOSFETs

*Source: rusEFI Discord, 2025-11-25 | Channel: epicEFI dev (1356732771325968630)*

*Contributors: Robb235 (robb235_00916), Joesphan (joesphan)*

## Summary

Unused ignition outputs on the uaEFI ECU cannot directly drive transmission solenoids because ignition drivers are not designed to source the constant high current that solenoids require. The correct approach is to use the ignition output as a logic-level signal to gate an external MOSFET, which then switches the solenoid load. A cheap Chinese PDM ("BigChungus") was suggested as a ready-made external switching solution adequate for solenoid duty.

## Details

### Why Ignition Outputs Cannot Drive Solenoids Directly

- Ignition coil drivers are peak-and-hold or high-side/low-side switches designed for **short-duration, high-current pulses** (ignition events), not sustained current draw.
- Transmission solenoids require **constant (or PWM) current** at levels that will overheat and potentially damage ignition driver ICs if used directly.
- Robb235 wanted to control three A340F transmission solenoids using three free ignition channels on uaEFI via a Lua script.

### Recommended Approach: External MOSFET

- Wire the ignition output pin as a **gate drive signal** to an N-channel MOSFET.
- The MOSFET source-drain path then switches the solenoid coil on/off.
- This isolates the uaEFI ignition driver from solenoid current and allows PWM control at appropriate duty cycles.

### "BigChungus" Chinese PDM Alternative

- A low-cost Chinese PDM board (colloquially called "BigChungus" or "CHUNGUS") contains multiple MOSFET-based output channels and can be driven by logic signals.
- Joesphan noted these devices use **counterfeit MOSFETs** with roughly 10× the rated R_DS(on) in practice.
- Despite that, they are considered adequate for solenoid switching (not high-current fan loads).
- Power dissipation estimate at 10 A DC through a fake MOSFET with ~0.22 Ω R_DS(on):
  - P = I² × R = 100 × 0.022 = **~2.2 W** — warm but not destructive.
- Recommendation: **do not use for radiator fan** or other high-sustained-current loads due to elevated R_DS(on) and thermal risk.
- Multiple channels can be paralleled to share current if needed.

### Lua Scripting Note

- Robb235 planned to control the A340F solenoids from Lua, which is a valid approach.
- Ensure the Lua script fits within the available script memory on the target ECU (see separate GDI8/Lua space documentation).
