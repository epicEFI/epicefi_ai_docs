# OEM ECU Torque Management in Automatic Transmissions

*Source: rusEFI Discord, 2026-02-20 | Channel: 1392172039430738111*
*Contributors: @ggurov, @MartyMotorsports, @Neos6443*

## Summary

OEM ECUs in modern vehicles manage torque reduction for automatic transmission gear shifts through a torque-demand architecture rather than direct actuator commands. In non-hybrid vehicles, spark retard is the primary mechanism for rapid torque reduction during gear changes. Hybrid vehicles have additional tools (BISG/CISG/CIMG motor-generators) that can apply or absorb torque very quickly, enabling smoother and faster shifts.

## Details

### Torque-Demand Architecture

In modern engine management systems, the accelerator pedal does not directly command a throttle angle. Instead, the pedal position is interpreted as a **torque demand**. The ECU then calculates how to achieve that requested torque using a model that determines the appropriate combination of:

- Throttle body opening (air charge)
- Fuel injection quantity
- Ignition timing

When torque reduction is needed (e.g., during an upshift), the ECU simply requests a lower torque target. All downstream actuators respond accordingly via the model, rather than being commanded individually.

### Torque Reduction Methods for Gear Shifts

**Spark retard** is the preferred method for rapid torque reduction during a gear shift in non-hybrid vehicles:

- Air charge reduction via throttle closing is too slow because of intake manifold filling/emptying lag.
- Variable valve timing (VVT/cam phasing) is similarly too slow to be useful within the duration of a gear change.
- Retarding ignition timing is nearly instantaneous and can sharply reduce torque output cycle-by-cycle.

**BISG / CISG / CIMG** (Belt-Integrated Starter-Generator / Crank-Integrated Starter-Generator / Crank-Integrated Motor-Generator) are used in mild-hybrid and hybrid systems:

- During upshifts: the motor-generator applies **negative torque** (load) to the engine, reducing engine speed without requiring the engine to unload.
- During downshifts: the motor-generator applies **positive torque** (assistance) to raise engine speed faster than the engine can do alone.
- In theory, these systems can hold the engine at the precise RPM required for smooth gear engagement, eliminating the RPM mismatch that would otherwise cause a jolt.

### Key Takeaways for rusEFI / Custom ECU Implementations

- Implementing shift torque management without a full torque model requires treating spark retard as the primary lever.
- A torque model (mapping requested torque to throttle position, ignition advance, and fuel) is a prerequisite for clean OEM-style shift management.
- Motor-generator-assisted shifting is only practical on vehicles that already have an integrated starter-generator on the belt or crank.
