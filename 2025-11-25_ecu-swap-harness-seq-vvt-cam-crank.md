# ECU Swap Harness Strategy: Adding Sequential Injection, VVT, Cam and Crank Sensors

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: Mr. Zebra (@mr.zebraaa)*

## Summary

When migrating from an older ECU to a new one (such as epicEFI) on an engine that originally ran a simpler system, the question is whether to swap the entire harness or extend the existing one. The practical approach is to add wires to the existing harness for new signals rather than replace the entire harness — specifically to wire in sequential injection, VVT, camshaft position, and crankshaft position sensors that the original ECU did not use.

## Details

**Scenario:** Engine previously ran on an old ECU and the harness was functional. New ECU supports additional features (sequential injection, VVT, cam/crank sensors) that the old harness did not wire up.

**Recommended approach:**
- Do not replace the entire harness if the existing one is in good condition
- Add the missing signal wires as extensions or splice-ins to regain functionality the new ECU requires
- Review the new ECU's pinout diagram before starting to identify exactly which signals need new wire runs

**Signals to add when upgrading to sequential/VVT-capable ECU:**

1. **Sequential injection** — requires individual injector signal wires for each cylinder in firing order. If the old setup was batch/bank fire, only 1–2 injector signals existed; sequential needs one wire per injector.

2. **Variable Valve Timing (VVT)** — requires:
   - VVT control output wire to the VVT solenoid
   - Cam position sensor input (if not already present)

3. **Camshaft position sensor** — digital or analog input depending on sensor type; needs signal, ground, and possibly 5V supply run to it

4. **Crankshaft position sensor** — typically already wired if the engine was running; verify the signal wire is connected to the correct input pin on the new ECU

**Key recommendation:** Consult the pinout diagrams and wiring schematics for the specific ECU hardware variant being installed before deciding on the harness strategy. Pin assignments vary between ECU models.
