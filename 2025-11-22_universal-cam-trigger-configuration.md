# Universal Cam Trigger Configuration in epicEFI / rusEFI
*Source: rusEFI Discord, 2025-11-22 | Channel: 1356732771325968630*
*Contributors: @ggurov (ggurov), @Tera (teraflopping)*

## Summary

Documents the universal cam trigger feature in epicEFI/rusEFI, which allows configuring a cam sensor trigger pattern entirely through the UI (gap ratios, tooth counts, sync edges, TDC position) without recompiling firmware. Includes the `initializeUniversalCam2` implementation, an important safety warning about bricking the ECU, and guidance on using universal trigger to iterate before committing to a hardcoded pattern.

## Details

### Overview

The universal trigger system allows users to define a cam trigger wheel pattern entirely through configuration tables rather than hard-coded firmware. This is especially useful when:
- Iterating on a new trigger wheel definition (avoids reflashing firmware on every change)
- Translating an OEM trigger pattern into rusEFI trigger definitions
- Testing offsets, angles, and gap ratios before writing them into the codebase

ggurov described the workflow:
> "i'd actually prefer if you try to get it done with universal trigger, so if there's stuff missing there we can add it — cause then we can translate it to real trigger — a much much easier way to iterate"

### UniversalCam1 Code

The `initializeUniversalCam1` function uses a fixed `RiseOnly` sync edge and a `tdcPosition` of 0:

```c
void initializeUniversalCam1(TriggerWaveform* s) {
    s->initialize(FOUR_STROKE_CAM_SENSOR, SyncEdge::RiseOnly);
    s->tdcPosition = 0;

    for (int i = 0; i < config->universal_cam1_gaps && i < UNIVERSAL_CAM_MAX_TEETH; i++) {
        s->setTriggerSynchronizationGap3(i, config->universal_cam1_gap_from[i], config->universal_cam1_gap_to[i]);
    }
    // continues iterating through tooth configuration table...
}
```

The function iterates through configuration tables for gap ratios and tooth definitions, meaning the full pattern is defined in the UI-accessible config rather than in C code.

### UniversalCam2 Code

`initializeUniversalCam2` adds configurable sync edge and configurable TDC position, making it more flexible than UniversalCam1:

```c
void initializeUniversalCam2(TriggerWaveform* s) {
    s->initialize(FOUR_STROKE_CAM_SENSOR, (SyncEdge)config->universalTrigger2SyncEdge);
    s->tdcPosition = config->universalTrigger2TDC;
    // ...
}
```

- `universalTrigger2SyncEdge` — configurable rise/fall/both sync edge
- `universalTrigger2TDC` — configurable TDC position in degrees

This variant was added specifically to allow testing TDC position offsets (e.g., 540° vs 0°) without recompiling.

### Cam Rotation vs. Crank Degrees

When entering TDC position for a cam-based trigger, note the relationship between cam rotation and crank degrees:
- **1 cam rotation = 360° cam = 720° crank** (4-stroke cycle)
- The universal cam trigger table represents two cam rotations (one full 4-stroke cycle)
- The tooth table should end at 720 (crank-equivalent degrees) for a complete cycle

ggurov noted that a pattern ending at 360 can compile correctly if interpreted as two cam rotations. The UI should ideally enforce this, but a note about the 720-degree cycle expectation is worth adding to the UI.

### CRITICAL: ECU Brick Warning

ggurov issued an explicit safety warning when using the universal trigger feature:

> "just word of caution, if you enable it, and it errors out, do not reboot with the trigger selected — bricks the ECU"

**Safe procedure if the universal trigger errors:**
1. **Do not reboot** while the erroring trigger is still selected
2. Unset/disable the cam wheel assignment
3. Reboot the ECU
4. Then adjust offsets, tooth counts, or gap ratios as needed
5. Re-enable and test

The same risk applies when adding a custom trigger to the firmware code that errors on initialization.

### Configuration Elements

The universal trigger is configured through these UI-accessible parameters (for UniversalCam2):
- **Sync edge**: rise-only, fall-only, or both edges
- **TDC position**: degrees offset from the synchronization tooth to TDC
- **Gap from/to tables**: defines the tooth-gap ratio windows for synchronization
- **Teeth table**: rising/falling edge positions across the cam cycle

### Workflow: Universal Trigger to Hard-coded Pattern

Recommended development workflow for a new engine/trigger wheel:
1. Use universal trigger to iterate on gap ratios, tooth positions, and TDC offset
2. Once stable, translate the working values into a proper `initializeSomeTrigger()` C function
3. Submit the new trigger pattern to the firmware codebase

This avoids the slow compile-flash-test loop during initial development.
