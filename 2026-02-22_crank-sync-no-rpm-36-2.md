# Crank Signal Present But No RPM: 36-2 Trigger Sync Troubleshooting

*Source: epicEFI Discord, 2026-02-22 | Channel: 1356732771325968630*
*Contributors: @Robb235, @ggurov, @Joesphan*

## Summary

A user had a crank signal being received by epicEFI but no RPM was displayed. The engine used a 36-2 trigger wheel with 1 cam sensor — the same pattern as another known-working vehicle. The root cause was that trigger phase overrides were disabled in the tune, preventing sync. Enabling the overrides resolved the issue immediately.

A separate but related question arose about why the 36-2 sync gap ratio was failing, which was investigated and explained in detail.

## Details

### Symptoms

- Crank signal present and tooth counter incrementing correctly
- No RPM displayed
- Joesphan's diagnosis: "Look at sync" / "Getting a positive edge but no sync"
- ggurov's diagnosis: "the phase is jacked" — the ECU does not know where 1 revolution starts

### Root Cause: Disabled Phase Overrides

The user had used another user's tune as a base. In that base tune, trigger phase overrides were set but were turned off (disabled). The overrides are needed for sync on this pattern.

- Fix: Enable the phase overrides in the tune (they were already configured with the correct values, just switched off)
- Result: "he's off to the races now"

### 36-2 Gap Ratio Analysis (Supplementary)

While investigating, ggurov queried Claude Code about the expected gap ratio for a 36-2 trigger wheel. The analysis (from `trigger_universal.cpp`):

- **Primary sync gap ratio**: 3.0 (set via `setTriggerSynchronizationGap(skippedCount + 1)` where skippedCount=2)
- **Acceptable primary range**: 2.25 to 3.75 (±25% tolerance)
- **Secondary gap ratio**: 1.0
- **Acceptable secondary range**: 0.75 to 1.25

The user's observed tooth ratios were: `1 1 1 1 1 3.45–3.7 0.36–0.45 1 1 1 1`

The primary gap (3.45–3.7) fell within the acceptable range. However, the tooth immediately after the gap (0.36–0.45) fell below the secondary gap acceptance window of 0.75–1.25, causing sync failure.

**Why the post-gap tooth ratio is low:**
- Slightly uneven tooth spacing near the gap
- VR sensor signal conditioning affecting edge timing near the gap (weak signal on first tooth after gap)
- Sensor picking up a partial tooth or ringing

**Workaround**: Use the trigger gap override to widen the second gap acceptance window to accommodate the hardware's actual signal characteristics.

### SD Card Error on Startup (Same Session)

The same user was also getting an error when cycling the key. ggurov noted:
- The error may be related to having no SD card installed and recent firmware changes to SD card handling
- The error appears intermittently (not every key cycle)
- ggurov characterised it as possibly being a reset: "maybe it's a reset"
- Action: investigate whether missing SD card causes the startup error; workaround is to install an SD card
