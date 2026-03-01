# Spool Fuel Target Maps for Boost Transition Fueling

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: fast335xi (@fast335xi)*

## Summary

Turbocharged applications in rusEFI/epicEFI use a spool fuel target map to smoothly transition the lambda target when the engine transitions from cruise (lambda 1.0) into boost. Without it, the step change from the base fuel map to the load-based enrichment target can create a sharp fueling transient during spool-up.

## Details

**Base fuel map target:**
The base fuel map runs at lambda 1.0 continuously — this is the stoichiometric cruise strategy.

**Full-load enrichment target:**
When the engine reaches its load target (boost threshold), the fueling strategy switches to a richer target of approximately **lambda 0.84** for power enrichment and charge cooling. The exact threshold and target value are tunable.

> "If you're hitting load target then it riches up to 0.84" — fast335xi

**The problem — sharp transition:**
The step change between lambda 1.0 (cruise) and lambda 0.84 (full load) without any interpolation creates an abrupt fueling shift during turbo spool-up. This can manifest as a stumble or lean/rich spike as boost builds.

**Spool fuel target map — the solution:**
The spool fuel target map is an intermediate table that interpolates the lambda target during the transition period as boost builds. Rather than snapping directly from 1.0 to 0.84, the map smooths the progression over the spool phase:

> "Then there's spool fuel target maps which basically smooth out that transition" — fast335xi

**Configuration note:**
The distinction between the base fuel map (lambda 1.0) and the spool/boost enrichment is configured separately. Finding the correct configuration location for this behavior was noted as non-obvious:

> "All the time yeah it's annoying to find the configure to make it not do that sometimes" — fast335xi

This suggests the spool fuel target map is a distinct configuration section from the main fuel map, and users transitioning from simpler ECUs may initially miss it.
