# Fuel Cut Code 2, MAP Sensor Failure, and Cam Trigger Diagnosis

*Source: rusEFI Discord, 2026-03-01 | Channel: 1477527653186535625*
*Contributors: @ggurov, @Bearded_Bogan, @Joesphan*

## Summary

A "fuel cut code 2" in rusEFI/epicEFI indicates that a safety feature triggered fuel cut — specifically "Code 2: Settings" means a required feature is disabled in configuration. In the session documented here, the immediate trigger was an overboost condition caused by a failed MAP sensor reading ~300 kPa, combined with fuel/ignition features that had been disabled during earlier testing. The concurrent cam trigger issue was diagnosed using the high-speed trigger logger.

## Details

### Fuel Cut Code 2: Feature Disabled

The error message format is:

```
Code 2: Settings — Feature disabled in settings
```

This code means the ECU attempted to use a feature that has been turned off in the configuration. In this case, the fuel output and a related ignition feature had been explicitly disabled during a no-plug cranking test session. When those features are disabled, the ECU reports Code 2 if a condition that depends on them is triggered.

**Resolution**: re-enable the required feature(s) in the Settings page. Common features that generate this error when disabled: fuel injection, ignition output, idle control output.

### Overboost from Failed MAP Sensor

Simultaneously, the MAP sensor had failed and was reading approximately 300 kPa (~3 bar absolute) at idle/cranking. This caused the ECU to interpret an immediate overboost condition, triggering boost-protection fuel cut. The MAP sensor failure occurred after a previous run; the last known-good reading was approximately 60 kPa at idle (naturally aspirated / light throttle).

**Symptom**: MAP reads an implausibly high value (300 kPa) at cranking with no turbo operating.
**Effect**: overboost safety cut activates immediately on crank.
**Resolution**: replace or repair the MAP sensor before attempting to run. Running with a failed MAP sensor will prevent proper fueling under any conditions.

### Cam Trigger Diagnosis Using the Trigger Log

With cam sync unresolved (engine would not fully synchronize), the high-speed trigger logger in TunerStudio / rusEFI was used to diagnose the cam input:

**How to capture a trigger log**:
1. Open the trigger logger (high-speed logger page in TunerStudio).
2. Hit Start — no special inputs need to be selected, the trigger log captures all trigger events automatically.
3. Crank the engine (do not need to start it).
4. Stop the log and review.

**Key channels in the trigger log** (viewable in MegaLogViewer / MLV):
- `triggerPrimaryRise` — crank tooth events (60-2 or other crank wheel pattern)
- `vvtEventRiseCounter` — cam tooth events (intake and/or exhaust VVT cam signals)

In the log reviewed, `triggerPrimaryRise` showed expected crank teeth but `vvtEventRiseCounter` showed no cam events at all — confirming zero cam signal rather than a noisy or incorrectly-phased signal. With wiring confirmed good, the next diagnostic step was to verify ECU pin assignments (pins 72 and 94 for Hall sensor inputs 1 and 2 on this particular harness).

### Interface Note: Red Lines in the rusEFI/epicEFI UI

Red lines visible in the trigger logger or live data view are error indicators. They are not channel data — they represent error events logged by the ECU firmware. When troubleshooting trigger sync, red lines in the trigger log indicate trigger decoder errors (noise, missing teeth, sync loss events).

### MAP Sensor Wired to Wrong Input (Magnetic Sensor on Analog Input)

A related issue surfaced in the same session: a spare sensor, when passed metal objects, responded to the magnetic field rather than to pressure. This indicates that a magnetic/reluctor-type sensor (e.g. a VR or Hall-effect speed/position sensor) had been wired to an analog voltage input intended for a pressure sensor. The two sensor types are electrically incompatible and must not be substituted.

**Identification**: if a sensor responds to metal being waved near it rather than to applied pressure or vacuum, it is a magnetic sensor (VR/Hall), not a pressure transducer.

## Notes

- The trigger log is the single most useful tool for diagnosing cam/crank sync issues. It should be the first step after confirming wiring continuity.
- Do not attempt to tune or diagnose fueling with a failed MAP sensor — the load calculation is wrong for every cell, making any data from that session invalid.
- Trigger input logging in the high-speed logger captures all active trigger inputs regardless of which channels are displayed on screen; you do not need to pre-select individual inputs before cranking.
