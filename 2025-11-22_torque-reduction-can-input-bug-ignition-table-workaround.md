# Torque Reduction via CAN Input: Bug Report, Fix, and Ignition Switch Table Workaround
*Source: rusEFI Discord, 2025-11-22 | Channel: 1439273357424988301*
*Contributors: @sansvirus. (sansvirus.), @ggurov (ggurov)*

## Summary

The "torque reduction button" CAN input option in rusEFI/epicEFI has never functioned correctly — selecting it causes an immediate error. A fix was built and confirmed working on the Mega144F7. However, even with the input wired correctly, the underlying torque reduction feature (ignition retard triggered by the torque reduction button or nitrous button) is unfinished: the tables use gear as the Y-axis but only have 2 bins, and the feature has never been tested. The practical workaround for shift-cut ignition retard is to use an **ignition switch table** activated by a physical or virtual switch, with blend mode disabled so the switch table applies 100%.

## Details

### Bug: CAN Input for Torque Reduction Button

**Symptom:** Selecting "CAN input" as the source for the torque reduction button immediately produces an error in the UI. The error is visible on the settings screen as soon as the option is chosen.

**Root cause:** This is a bug inherited from the upstream rusEFI codebase. The CAN input option for the torque reduction button (and the nitrous button) has **never been implemented or tested** — ggurov confirmed "NOBODY has ever used that option, it's never worked."

**Fix:** A patched build was compiled by ggurov and provided to the user. The fix survives a burn/reload cycle (initial testing showed it reverted on burn, which was then resolved). Confirmed working on:
- **Board:** Mega144F7
- **Build date tested:** 20112025 (user's original build)
- **Fixed build:** compiled 2025-11-22

### Torque Reduction Feature Status (Post-Fix)

Even after the CAN input error is fixed and the button can be activated:
- The ignition retard does **not** fire as expected.
- Code inspection shows the torque reduction tables use **GEAR as the Y-axis** with only **2 bins**, shared with GPPWM1 infrastructure.
- The feature is considered **unfinished** — ggurov: "this needs to get finished better."
- The nitrous torque reduction button has the same untested status in upstream rusEFI.

### Workaround: Ignition Switch Table

Since the torque reduction retard feature is incomplete, the recommended workaround for applications needing gear-shift ignition cut or torque reduction is to use the **ignition switch table** feature:

1. Configure a physical switch input (or virtual switch via CAN) in epicEFI.
2. Assign that switch to trigger an **ignition table switch** (a second ignition advance table).
3. Set **blend mode = false** on the switch table — this causes the ECU to use the switch table at **100% authority** rather than blending it with the base table.
4. Populate the switch ignition table with the desired retard values (e.g., heavy retard for shift-cut).

The switch table approach was confirmed to work in testing by sansvirus. (2025-11-22 17:33).

**Screenshots referenced in Discord:**
- `image.png` (msg 1441837495451914362) — ignition switch table configuration screen
- `image.png` (msg 1441837558995615974) — blend mode setting for the switch table

### Key Settings

| Setting | Value | Notes |
|---|---|---|
| Torque reduction input source | CAN input | Requires patched build; underlying feature still incomplete |
| Ignition switch table | Enabled | Workaround for torque reduction / shift cut |
| Blend mode | False | Switch table applies at 100%, no blend with base table |
