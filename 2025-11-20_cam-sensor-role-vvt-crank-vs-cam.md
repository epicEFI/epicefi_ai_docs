# Cam Sensor Role Without VVT, and Crank vs. Cam Sensor Requirements

*Source: rusEFI Discord, 2025-11-20 | Channel: 1356732771325968630*
*Contributors: Mr. Zebra (@mr.zebraaa), EngineWhiz (@enginewhiz)*

## Summary

Clarification of what the camshaft position sensor is used for when VVT is absent, and whether a crankshaft position sensor is strictly required when the cam wheel has sufficient tooth count.

## Details

**Cam sensor without VVT:**
When an engine does not have variable valve timing (VVT), the camshaft position sensor serves a single purpose: providing engine cycle synchronization (determining which stroke the engine is on — compression vs. exhaust TDC). It does not contribute to high-resolution timing in this case; that role belongs to the crank sensor.

**Running on cam sensor only (no crank sensor):**
It is possible to run an engine using only the camshaft trigger wheel, with no dedicated crankshaft position sensor, provided the cam wheel has sufficient tooth count for accurate timing resolution. The threshold mentioned in this discussion is more than 60 teeth on the cam wheel, at which point:

- The cam wheel alone provides enough angular resolution for precise ignition and injection timing.
- No separate crank sensor is required.
- The cam signal inherently provides both sync and position at the cost of some noise sensitivity (cam drive has more compliance than the crank).

**Practical guidance:**
- On engines with a low-tooth-count cam sensor (e.g., 1 tooth per revolution) and no VVT, the cam is used only for the synchronization pulse — the crank wheel handles all timing resolution.
- On engines with a high-density cam trigger (60+ teeth), the cam wheel alone can serve both functions (sync and timing).
- In rusEFI, trigger decoder selection should match the actual sensor setup; using a cam-only decoder is supported when the wheel geometry meets the resolution requirement.
