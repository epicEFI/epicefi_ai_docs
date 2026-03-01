# Startup No-Spark Diagnosis: "Wait for Sync" and Coil Hardware Test

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: Tera (@teraflopping), ggurov (@ggurov), Joesphan (@joesphan)*

## Summary

When an engine cranks but does not fire, "Wait for Sync" is a common cause: the ECU holds off fuel and spark for up to 4 crankshaft revolutions until it achieves cam/crank synchronization. Disabling this setting allows the ECU to begin attempting injection and ignition immediately on cranking. A hardware test (firing coils individually) confirmed coil function and also caused the engine to spin slightly — a useful diagnostic indicator.

## Details

### Symptom

Engine cranks but produces no spark. Injectors confirmed firing via hardware test (scope / HW test mode). No fuel or ignition events observed during normal cranking attempts.

### Root Cause: "Wait for Sync"

When "Wait for Sync" is enabled, the ECU will not output fuel injection or ignition events until it has decoded at least one full sync pattern from the crank/cam trigger. This typically takes up to 4 engine revolutions after cranking begins.

- If the engine is not starting and the crank sensor signal is intermittent, noisy, or simply taking longer than expected to decode, the ECU will sit silently and not fire at all.
- The ECU may be actively trying to sync (indicated by the TunerStudio status showing "trying to ignite") even though no output events fire yet.

**Fix**: Disable "Wait for Sync" in the trigger/ignition settings. With it off, the ECU will begin firing injection and spark outputs immediately, even before full trigger sync is achieved.

### Hardware Test Procedure

epicEFI provides a hardware test mode that activates individual outputs in isolation:

1. Enter hardware test mode in TunerStudio.
2. Fire injectors individually — confirm each injector pulls low (scope shows pulse to ground).
3. Fire coils individually — confirm each coil produces a spark.

In this session: injectors were confirmed hitting. Coil 5 when activated caused the engine to spin slightly — a normal reaction when a single coil fires with fuel present in that cylinder.

Other coils that did not respond required further investigation (separate issue from "Wait for Sync").

### Distinction: Coils vs Injectors in Hardware Test

When probing with a scope during a hardware test:
- **Injector** output: Yellow wire (in this vehicle's harness) — pulls to ground when active.
- **Fuel pump** output: Blue wire — separate signal, not injector.

Mixing these up will give misleading scope results. Confirm wire colors against the harness before interpreting scope captures.

### Additional Note: Onboard Sensors During Faults

If the ECU is actively throwing fault codes, onboard sensors (MAP, TPS, etc.) may be reported as fixed/stuck values (e.g., pinned at 1) rather than live readings. Do not rely on those sensor readings for diagnosis while faults are active.
