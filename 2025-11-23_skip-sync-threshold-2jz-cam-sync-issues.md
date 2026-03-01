# Skip-Sync Threshold Configuration and 2JZGTE Cam Sync Issues

*Source: rusEFI Discord, 2025-11-23 | Channel: 1405892994506424421*
*Contributors: Joshan Lu (@joshseek_30236_07984), Joesphan (@joesphan), ggurov (@ggurov)*

## Summary

Joshan Lu documented testing of the skip-sync feature in rusEFI/epicEFI and identified several related issues with cam synchronization on a 2JZGTE engine: incorrect sync when cranking at low RPM thresholds, no visual RPM reset in TunerStudio when the skip-sync threshold is crossed, DSC (VR conditioner) interference with the cam signal, and loss of sync above 3500 RPM. Testing concluded that the uaEFI hardware cannot reliably process the 2JZGTE VR cam signal.

## Details

### Skip-Sync Feature

The skip-sync feature prevents the ECU from re-synchronizing (re-acquiring cam/crank phase lock) once the engine RPM exceeds a defined threshold. This avoids false re-sync events at speed caused by noise or momentary signal glitches.

**Configuration tested by Joshan Lu:**
- Skip-sync threshold: **1500 RPM**
- Behavior: Once RPM reaches or exceeds 1500 RPM, the ECU will not attempt to re-sync.
- Joshan shared a configuration file (`skip_sync.rar`) demonstrating this setup.

### Issue 1: Incorrect Cam Sync When Cranking Below Threshold

When the skip-sync threshold was set to 500 RPM (or when cranking from rest to 500 RPM), the engine would start with **incorrect cam synchronization**:

- The engine would crank and fire, but with the wrong cam phase established.
- Joshan Lu identified this as a cam sync issue: *"it will crank and start with the wrong sync too, which leads me to believe its to do with cam sync"*.
- Root cause: At very low cranking RPMs, the cam sync tooth may be misidentified or the sync algorithm establishes phase on the wrong tooth. This is consistent with known single-tooth cam sync sensitivity at low speeds.
- Joesphan confirmed: *"its always the single tooth cam"* — single-tooth cam sync is inherently more susceptible to incorrect phase capture at cranking speeds.

### Issue 2: No Visual RPM Reset in TunerStudio at Threshold Crossing

With skip-sync set to **1500 RPM** and engine revved to **2000 RPM**:
- **No visual RPM reset to 0** was observed in TunerStudio (TS) when crossing the 1500 RPM threshold.
- However, **slight exhaust blipping** was audible, indicating the ECU was responding to the threshold event internally even if the TS display did not reflect it.
- This suggests the skip-sync boundary is being processed in firmware but may not trigger a visible TunerStudio gauge event; the display behavior is a cosmetic observation, not an indication the feature is non-functional.

### Issue 3: DSC (VR Conditioner) and Cam Signal Interference

When the DSC (a VR signal conditioner used in this installation) was routed through the cam signal path:
- The car **would not start** with DSC engaged in the cam signal circuit.
- Joshan Lu: *"Car won't start with the dsc in I think it probably wrecks the cam signal"*.
- The DSC conditioner appeared to corrupt or suppress the cam sync signal, preventing the ECU from establishing sync during cranking.
- Moving the DSC off the VR input jumper (as Joshan noted: *"I should really put it on the jumper not the VR inputs"*) resolved the no-start condition.
- After removing DSC from the cam path, the engine started successfully. Joesphan reviewed logs and confirmed: no cut codes, trigger errors clean.

### Issue 4: Lumpy Fueling at Idle

After resolving the cam signal issue, Joesphan identified **lumpy fueling** in the logs:
- Recommended increasing **idle TPS** slightly to provide more air at idle.
- *"more air more smooth"* — lean idle was contributing to rough idle, not a trigger issue.

### Issue 5: Sync Loss Above 3500 RPM / Off AFRs

At higher RPM:
- Engine **lost synchronization above approximately 3500 RPM**.
- **AFRs were significantly off** at higher loads.
- Two candidate causes discussed: (a) poor tune (VE table / ignition timing not calibrated), (b) hardware inability to process the 2JZGTE VR cam signal at speed.

**Resolution:** Joshan Lu concluded that **uaEFI hardware cannot reliably run the 2JZGTE VR cam signal** at higher RPMs:
- *"We may now conclude that uaefi cannot run a 2jzgte vr"*
- *"Mystery solved thanks to all"*
- Testing continued to optimize timing; Joshan noted timing ended up at **402 degrees** using the G2 cam configuration.
- The AFR/sync issues at high RPM were attributed to a combination of the hardware limitation and needing proper VE/ignition table calibration. Joshan noted the base maps used were ggurov's bench H7 maps (not 2JZ-specific).

### Timing Configuration Note

- Trigger offset for this 2JZGTE setup settled at **402 degrees** using the G2 cam decoder.
- Initial testing used the DSC conditioner; later testing switched to MAX9926 for the cam signal path.

### Summary of Findings

| Issue | Root Cause | Resolution |
|---|---|---|
| Wrong sync at crank (500 RPM threshold) | Single-tooth cam sync unreliable at low RPM | Raise skip-sync threshold; do not crank below threshold |
| No TS RPM reset visual at 1500 RPM | Display behavior only; feature functional | Confirmed by exhaust blip; cosmetic issue |
| No-start with DSC on cam signal | DSC conditioner corrupting cam VR signal | Remove DSC from cam path; use correct VR conditioner |
| Sync loss above 3500 RPM | uaEFI hardware cannot process 2JZGTE VR cam at speed | Use compatible hardware (Proteus / boards with proper VR input) |
| Off AFRs | Base maps not tuned for this engine | Requires proper VE and ignition calibration |
