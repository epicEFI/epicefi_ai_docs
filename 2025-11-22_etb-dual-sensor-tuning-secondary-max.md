# ETB Dual Sensor Calibration: Secondary Max, Redundancy Modes, and Single-Sensor ("Suicide ETB") Workaround
*Source: rusEFI Discord, 2025-11-22 | Channel: 1401895238481744052*
*Contributors: @Robb235 (robb235_00916), @Joesphan (joesphan), @ggurov (ggurov), @Walter R. (walterronny)*

## Summary

When running an Electronic Throttle Body (ETB) with redundant dual-sensor inputs (PPS1/PPS2 and TPS1/TPS2), the "Secondary Max" percentage setting for each sensor pair is extremely sensitive and must be calibrated precisely. A specific Toyota DBW design where VTA2 (TPS2) reaches its maximum voltage at approximately 70 degrees of throttle angle (not 70% open) — well before wide-open throttle — is not natively handled by the firmware's dual-sensor validation, causing stuttering and false limp-mode triggers. Three workarounds are available in epicEFI/rusEFI for these non-standard redundant sensor designs.

## Details

### Background: Toyota Dual-Sensor ETB Behavior

Toyota's early DBW design includes a **magnetic clutch failsafe**: if the ETB controller detects a fault, the clutch disengages and a cable mechanically opens the throttle to approximately 45–50% (a hardware limp mode). The dual-sensor design uses:

- **VTA1 (TPS1)** — primary throttle position sensor, linear 0–100%.
- **VTA2 (TPS2)** — redundant sensor that reaches its **maximum voltage at approximately 70 degrees of throttle angle**, not at actual WOT. This is a deliberate Toyota design choice.
- **PPS1 / PPS2** — dual pedal position sensors. PPS2 similarly hits maximum voltage before actual WOT.

The firmware's redundancy check compares primary and secondary sensor readings and triggers limp mode if they diverge beyond a threshold. Because VTA2 and PPS2 saturate at ~65–70% of travel, the firmware's default behavior does not account for this and treats the saturated secondary readings as a divergence fault.

### Symptom: Stuttering at ~45% PPS

When `TPS Secondary Max` or `PPS Secondary Max` is set even slightly above the true maximum the secondary sensor can report:
- TPS and PPS readings **bounce between 0% and ~40%** once the pedal reaches ~45% PPS.
- This appears as a stutter in ETB actuation; confirmed via log and video.
- ETB motor drive frequency (tested 300 Hz to 1,200 Hz) has **no effect** on the stuttering — it is purely a sensor validation issue.
- The acceptable window for `TPS Secondary Max` can be as narrow as a few percent (e.g., 57% works, 60% does not).

### Workaround 1: Precise "Secondary Max" Calibration

Set `TPS Secondary Max` and `PPS Secondary Max` to the exact voltage ceiling of the secondary sensor, not 100%. For the Toyota VTA2 example, the secondary max must be set at the voltage/percentage the sensor actually reaches at full travel, which is substantially below 100%.

This requires careful sweep logging to identify the true peak of each secondary sensor and entering those values exactly.

### Workaround 2: "Ignore Sensors Too Close" / "Allow Identical PPS"

In the epicEFI tuning software, searching for **"no TPS"** (labeled "super dangerous" in the UI) reveals a set of redundancy override settings:

- **"Ignore TPS/PPS Too Close"** (`ignore sensors too close`) — disables the fault that fires when sensor readings are too similar to each other (would otherwise indicate a wiring short/fault). Useful when PPS1 and PPS2 are intentionally tied to the same physical sensor output.
- **"Allow Identical PPS"** — allows both PPS channels to read the same signal without triggering a redundancy fault.

These settings do **not** eliminate redundancy — the dual-sensor math still runs underneath; they only suppress specific false-fault conditions. The safety architecture remains intact.

### Workaround 3: Single-Sensor Mode ("Suicide ETB")

The setting **"run with one TPS/PPS"** (single PPS mode) instructs the firmware to read **only PPS1 and TPS1**, ignoring the secondary sensors entirely. This is nicknamed "suicide ETB" because it removes all hardware redundancy.

- Search for **"no TPS"** in the tune editor to find this setting.
- Once enabled, `Secondary Sensor Max` percentages become irrelevant.
- A known user (@pirate/Walter's Golf application) has been running single-sensor mode for an extended period including cruise control, without issues.
- This was specifically written to support applications where a non-standard secondary sensor cannot be made to work with the validation logic.

**Safety note:** Single-sensor mode eliminates the dual-sensor cross-check that is required for OEM safety compliance. Use only in applications where the mechanical failsafe (if present) or other safeguards are acceptable substitutes.

### "Ford/Toyota Redundant TPS Mode"

A separate setting labeled **"Ford/Toyota redundant TPS mode"** exists in the firmware. The suspected behavior is: ignore TPS2 readings and rely only on TPS1 once TPS1 exceeds a threshold percentage — matching the known Toyota VTA2 saturation behavior. Whether the implementation is fully functional was not confirmed in this conversation and requires code review.

### Summary of Available Settings

| Setting | Location | Effect |
|---|---|---|
| `TPS Secondary Max` / `PPS Secondary Max` | ETB sensor calibration | Set to actual peak voltage percentage of secondary sensor |
| Ignore TPS/PPS Too Close | Search "no TPS" in tune editor | Suppresses false fault when secondary reads same as primary |
| Allow Identical PPS | Search "no TPS" in tune editor | Allows both PPS channels to share the same signal |
| Single PPS mode ("suicide ETB") | Search "no TPS" in tune editor | Disables secondary sensor entirely, uses PPS1/TPS1 only |
| Ford/Toyota Redundant TPS Mode | ETB advanced settings | Reportedly switches to primary-only above a threshold % |
