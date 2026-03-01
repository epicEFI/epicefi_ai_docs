# Wastegate Operation and Exhaust Backpressure Diagnosis

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @Chemex-joshseek, @ToyotaTerrorist*

## Summary

A brief but useful tip was shared regarding diagnosing excessive exhaust backpressure in turbocharged systems: if the wastegate cannot stay closed, the root cause is likely excessive backpressure in the exhaust system. This was shared in context of planning a "screamer dump" (external wastegate atmospheric dump) exhaust fabrication.

## Details

### Wastegate Won't Stay Closed: Backpressure Diagnosis

A wastegate is normally held closed by spring pressure (and optionally boost pressure signal or electronic actuation) until the target boost pressure is reached. If the wastegate opens prematurely or cannot remain fully closed at low boost:

> "if the wastegate cant keep shut it means u have way too much back pressure"

**Causes of excessive backpressure:**
- Restrictive downpipe or exhaust piping
- Catalytic converter with excessive restriction
- Undersized exhaust piping diameter
- Tight exhaust bends with insufficient radius
- External wastegate dump pipe routing back into the main exhaust (increases backpressure at the wastegate outlet)

**Diagnosis steps:**
1. Check that the wastegate actuator spring rating is appropriate for your boost target.
2. Inspect the exhaust system for obvious restrictions or collapsed sections.
3. Consider whether the wastegate dump is recombined into the exhaust (common on street cars) — if so, relocate to atmosphere (screamer) for diagnosis.
4. Measure exhaust backpressure with a pressure sensor upstream of the turbine if the issue persists.

### Screamer Dump Fabrication

A "screamer dump" refers to routing the external wastegate outlet directly to atmosphere rather than back into the exhaust system. This:
- Eliminates backpressure at the wastegate outlet
- Allows the wastegate to open freely
- Is loud (hence "screamer") — typically not street-legal without noise attenuation

Fabrication requires a suitable flange to connect the wastegate outlet to the dump pipe. The specific flange size depends on the wastegate model being used.

### O2 Sensor Damage from Backfire

Related to exhaust system issues: backfires can damage O2 sensors:

> "my ke70 just wrecked the o2 sensors when it backfired"

A backfire sends a pressure pulse and potentially a flame front back through the exhaust, which can physically damage the wideband sensor element or contaminate it. After a backfire event, inspect O2 sensor readings for erratic behavior and replace if the sensor appears damaged.
