# Vanos Solenoid Behavior in Emergency Mode Causes Cam Overlap and Poor Idle

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: Tera (@teraflopping), fast335xi (@fast335xi)*

## Summary

When BMW Vanos solenoids default to "emergency mode" (i.e., the ECU is not actively commanding them), they cause significant cam overlap and produce a rough idle characteristic similar to a high-lift/long-duration camshaft. This is a diagnostic finding useful when commissioning a rusEFI/epicEFI install on a Vanos-equipped BMW engine.

## Details

**Observation:**

Tera measured the Vanos solenoid signals with an oscilloscope while the engine was running with the solenoids in their unpowered/emergency fallback state. The result was a large amount of cam overlap between intake and exhaust events — enough to produce a noticeably rough idle with a loping quality comparable to a heavily-profiled aftermarket camshaft ("HOG ASS CAM" in Tera's words).

fast335xi independently noted the same root cause: "This is just vanos overlap."

**Why this matters for rusEFI/epicEFI commissioning:**

If a Vanos-equipped engine idles poorly during early ECU bringup — before Vanos control is properly configured and outputting correct solenoid commands — the cause may simply be the solenoids sitting in their de-energized default position, which produces maximum overlap. This is not a fueling or ignition timing problem; it is a VVT control issue.

**Diagnostic checklist:**

- Measure the Vanos solenoid gate signals with an oscilloscope during idle to confirm they are being driven (not floating or de-energized).
- If solenoids are receiving no command signal, the ECU Vanos output configuration should be verified before diagnosing fueling or base timing issues.
- The characteristic rough/lopey idle from Vanos in emergency mode can be mistaken for a tuning problem but is actually a wiring/configuration issue.

**Engine context:**

This discussion was in the context of a BMW 750i V8 (likely N63 or S63 variant) running on epicEFI hardware, though the principle applies to any BMW engine with Vanos solenoids (e.g., DOHC inline-6 with double-Vanos, N54, N55, S65, etc.).
