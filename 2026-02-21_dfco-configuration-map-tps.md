# DFCO Configuration: TPS/APP + RPM Conditions and MAP Pin Workaround

*Source: epicEFI Discord, 2026-02-21 | Channel: 1401895238481744052*
*Contributors: @Tuga, @MykkH*

## Summary

Discussion around configuring Decel Fuel Cut-Off (DFCO) to trigger based on both throttle position (TPS/APP) and RPM, and a workaround for using MAP-based DFCO conditions when the MAP sensor pin is unassigned or shared.

## Details

### DFCO Trigger Conditions: TPS/APP and RPM

@Tuga asked whether DFCO can be configured to use both TPS/APP position and RPM as conditions. The stock rusEFI/epicEFI DFCO implementation supports enabling fuel cut based on throttle position and RPM thresholds. No definitive answer with specific menu paths was given in this thread, but the question confirms this is a desired/tested configuration.

### MAP Pin and DFCO: Workaround for Unassigned MAP

If the MAP sensor pin is unassigned (i.e., no physical MAP sensor is connected or the pin is intentionally left unassigned), the MAP-based conditions in the DFCO configuration become non-functional — DFCO will not use MAP pressure as a cutoff trigger.

**Workaround using "Allow Multiple Inputs on Same Pin"**:

@MykkH proposed a workaround for setups without a dedicated MAP sensor pin:

1. Enable the **"allow multiple inputs on same pin"** feature in rusEFI/epicEFI.
2. Assign both the MAP input and the TPS signal to the **same physical pin**.
3. This creates a "faux-MAP" input that allows the DFCO MAP condition to remain active, driven by TPS signal voltage.

This is useful in configurations where the MAP pin is repurposed or unavailable, and the user still wants MAP-threshold DFCO behavior to function without rewiring.

### Key Takeaways

- DFCO conditions based on TPS/APP and RPM are supported in rusEFI/epicEFI.
- Unassigning the MAP pin disables MAP-based DFCO conditions entirely.
- The "allow multiple inputs on same pin" feature can be used to assign MAP and TPS to the same pin, enabling DFCO MAP logic to function using TPS signal as a proxy.
