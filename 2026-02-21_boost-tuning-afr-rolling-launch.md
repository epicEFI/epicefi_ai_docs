# Boost Tuning Session: AFR, Fuel Cut, and Rolling Launch Setup

*Source: epicEFI Discord, 2026-02-21 | Channel: 1411925674498719775*
*Contributors: @Immrpablo, @ggurov, @jeybee*

## Summary

During a live boost tuning session, @Immrpablo was running dangerously lean AFR (14.4:1) under boost conditions while also triggering fuel cut codes. The discussion covered correct AFR targets under boost, throttle response issues linked to fuel cut, NA-mode treatment below 4500 RPM, and the need for a rolling launch setup to safely manage boost transitions.

## Details

### Observed Problems

- **Fuel cut codes active**: The ECU was triggering fuel cut, causing dead throttle feel when lifting.
- **Lean AFR under boost**: AFR was reading 14.4:1 under boost — dangerously lean for a forced induction setup. At 12 psi, AFR was already at 9:1, suggesting VE table is too rich at higher pressures despite needing richer targets generally under boost.
- **Throttle feel**: When reducing throttle, the response felt "dead" — a symptom consistent with active fuel cut codes interrupting fueling.

### NA Treatment Below 4500 RPM

The engine should be treated as naturally aspirated (NA) until approximately 4500 RPM. Aggressive ignition timing is appropriate in this rev range. Above 4500 RPM, boost comes on and tuning strategy changes accordingly.

### AFR / VE / Ignition Advance Observations

- AFR and VE targets need review; at 12 psi the mixture was already at 9:1 AFR, indicating the VE table may be overly rich at higher MAP pressures.
- Ignition timing was described as looking cleaner after adjustments, but the VE table remains a concern at elevated boost levels.
- Under boost conditions, AFR should be significantly richer than stoichiometric (14.7:1). Running 14.4:1 under boost is a critical calibration error.

### Rolling Launch

@ggurov recommended setting up a rolling launch to manage the boost transition properly. @Immrpablo needed guidance on how to configure this feature in rusEFI/epicEFI. A rolling launch controls engine output during vehicle roll to allow safe boost building before a full-throttle event.

### Key Takeaways

- Do not run lean AFR (14.4:1) under boost — this risks engine damage.
- Resolve fuel cut codes before tuning further under boost.
- Treat the engine as NA below 4500 RPM; apply aggressive timing in that range.
- Set up rolling launch to safely manage boost builds.
- Review VE table at higher MAP pressures — at 12 psi, AFR of 9:1 suggests the VE is already too rich and will worsen at higher boost.
