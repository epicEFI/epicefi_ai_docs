# ALS Fixed-Mode Timing Behavior and Trigger Offset Calibration

*Source: rusEFI Discord, 2025-11-21 | Channel: general-tuning (1401895238481744052)*

*Contributors: ggurov, SanGawku, Joesphan*

## Summary

Anti-Lag System (ALS) in fixed timing mode does **not** modify ignition timing in epicEFI, matching rusEFI behavior. A user migrating a tune from rusEFI to epicEFI found their trigger angle offset needed to change from 44° to 25°; this was eventually traced to a friend having left ALS enabled with dynamic timing active (not locked), resulting in accidental advance during a dyno pull. The correct method for trigger angle calibration is to lock timing in software and verify with a timing light. A planned "auto-find trigger offset" feature would scan through angles at crank until the engine fires.

## Details

### ALS Does Not Change Timing in Fixed Mode

- In epicEFI (and rusEFI), when timing is set to **fixed/locked mode**, the ALS system does not adjust the ignition angle.
- If timing is set to **dynamic mode**, ALS can add advance. Leaving ALS enabled with dynamic timing during a pull produced ~30° of extra advance in the incident below.
- Confirmed by ggurov: *"no, ALS does not change timing in fixed mode"* and *"yeah, fixed mode doesn't move with als"*

### Trigger Offset Discrepancy When Migrating from rusEFI to epicEFI

- User (SanGawku) noted trigger offset changed from **44°** (rusEFI, dyno-verified) to **25°** (epicEFI) to achieve correct timing with a timing light.
- ggurov confirmed this should not be the case: *"trigger code is basically 99% rus"* — the discrepancy was not an epicEFI firmware difference.
- Root cause: a third party had enabled ALS with dynamic timing active during the original pull, adding approximately **20–30° of unintended advance**. The rusEFI dyno tune was done with this extra advance in place, making the base offset appear different.

### 8-Tooth Distributor Wheel (No Home Tooth) Configuration

- Trigger wheel: custom wheel, 8 teeth, 0 missing (uniform spacing — distributor signal).
- No cam/home signal; single coil; batch injection (2 injectors per output).
- epicEFI configuration that worked: **falling edge detection**, **0° trigger angle advance**.
- ggurov: *"rising vs falling goes one or the other side of the tooth"* — edge selection shifts effective angle by one tooth width.

### Ignition Hardware Angle Correction

- ggurov asked about "ignition hardware angle correction" as a possible source of offset differences between firmware versions. Worth checking if this parameter exists and differs between rusEFI and epicEFI configs when migrating tunes.

### Timing Light Calibration Procedure

- Lock timing in software to a known value (e.g., 10°).
- Use a timing light on the crank pulley marks to confirm the physical spark timing matches the software value.
- Adjust trigger angle offset in software until the timing light confirms the correct angle.
- Joesphan's workflow: *"set that in software, lock at 0 or 10"* — the software value is the reference; the trigger offset is adjusted to make reality match software.
- Note: a dialback-style timing light is preferred for simplicity; advanced digital timing lights that compute advance independently can cause confusion.

### Planned Feature: Automatic Trigger Offset Scan

- ggurov described a planned "help me start this engine" feature:
  - On each crank attempt, retard the trigger angle advance by a set number of degrees.
  - Repeat (crank → step → crank → step) until the engine fires.
  - Once firing is detected, stop stepping and report the found offset in a dialog.
- Intended as a "get in the ballpark" tool, not a replacement for timing-light verification.
- Related docs: epicEFI documentation PDFs available at `https://content.epicefi.com/documentation/pdfs/epicECU%20documentation/Trigger%20Settings.pdf`
