# Lean Cruise Implementation via AFR Table and TPS Derivation from MAP

*Source: rusEFI Discord, 2025-11-22 | Channel: 1401895238481744052*
*Contributors: ggurov (@ggurov), MykkH (@mykkh)*

## Summary

Two related ECU tuning strategies: (1) implementing lean cruise by directly editing the AFR target table rather than using conditional modifiers, and (2) deriving a virtual TPS signal from MAP sensor readings when a throttle position sensor is not available or trustworthy.

## Details

**Lean Cruise via AFR table (quick method):**
The standard rusEFI/epicEFI "Lean Cruise" feature operates as a conditional modifier — it activates when specific conditions are met (speed, load, coolant temp, etc.) and overrides the AFR target. However, the simplest and most transparent approach is to directly set lean AFR targets in the relevant cells of the main AFR table:

- Target cells: **low load, mid RPM** region of the AFR target table.
- Set the desired lean AFR value (e.g., 15.0–16.0 for gasoline) directly in those cells.
- This avoids the complexity of tuning conditional modifier enable/disable conditions.
- MykkH notes: lean cruise conditions are typically based on met conditions rather than timers — the table-based approach gives the same end result without a separate feature to configure.

**Lean mixture and misfires:**
ggurov noted a practical limit: running too lean causes misfires. When pushing lean AFR targets, verify that the engine does not misfire at the target load/RPM points. If misfires appear, the target is too lean for that operating condition and the AFR cell should be richened slightly.

**TPS derived from MAP (virtual TPS):**
When a physical TPS is absent, unreliable, or needs cross-checking, rusEFI/epicEFI can calculate a substitute throttle position value from the MAP sensor reading:

- The ECU computes a TPS-equivalent value based on MAP, effectively faking the sensor input.
- This allows load-based fueling to continue using MAP as the primary load axis while still providing a TPS-like signal for features that require it.
- Useful during initial engine bring-up before a TPS is wired, or on carbureted/throttle-body setups where MAP is a better load indicator than TPS.
