# Vacuum System Configuration for VNT Actuation

*Source: rusEFI Discord, 2026-02-28 | Channel: 1411925674498719775*
*Contributors: @mnemonik*

## Summary

A vacuum circuit for controlling a Variable Nozzle Turbine (VNT) actuator uses a reservoir with an anti-return valve, in parallel with an N75-style non-leaking solenoid, feeding a PWM-controlled vacuum actuator on the VNT.

## Details

The described vacuum plumbing topology is:

```
Vacuum source
    |
    v
Anti-return (check) valve
    |
    v
Vacuum reservoir (orb/canister)
    |
    +-- [parallel] N75-style non-leaking solenoid valve
    |
    v
PWM-vacuum VNT actuator
```

Key points:

- An **anti-return (check) valve** between the vacuum source and reservoir prevents backflow when manifold vacuum drops (e.g., at WOT).
- A **vacuum reservoir** (orb/canister) stores vacuum so the actuator has consistent pressure independent of momentary manifold pressure changes.
- The **N75-style valve** must be a non-leaking type — a leaking valve would bleed down the reservoir and cause erratic VNT position control.
- The **VNT actuator** is controlled by PWM vacuum signal, allowing proportional nozzle position control rather than simple on/off.

## Notes

This configuration mirrors standard OEM boost control vacuum routing adapted for VNT-specific needs. The non-leaking requirement for the solenoid is critical — standard N75 valves that allow bypass flow when de-energized are not suitable here.
