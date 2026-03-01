# MS3 Features: High Power Enrichment Curve and Check Engine Light Logic

*Source: rusEFI Discord, 2025-11-18 | Channel: 1379478511667908618*
*Contributors: @ggurov*

## Summary

Notes on two MS3 (MegaSquirt 3) engine management features observed while reviewing the MS3 documentation: the "high power enrichment" curve for managing AFR during sustained high-load conditions, and built-in check engine light (CEL) logic based on min/max thresholds and fluctuation detection.

## Details

### High Power Enrichment Curve

MS3 includes a feature called the "high power enrichment" curve:

> "MS3 has a 'high power enrichment' curve. it will alter target afr after a certain time above a certain load, so high duration boost/open throttle runs" — @ggurov

This feature is designed to enrich the air-fuel ratio after the engine has been held above a defined load threshold for a sustained period. The two configurable parameters are:
- **Load threshold** — the MAP/TPS level above which the feature activates
- **Time above threshold** — how long the engine must remain at that load before the enrichment kicks in

The practical use case is protecting the engine during extended wide-open throttle or boost events (e.g., a long dyno pull or track straight). After the time threshold is exceeded, the target AFR shifts richer to add a thermal protection margin.

This is distinct from immediate load-based enrichment (like a standard power enrichment table) — it specifically targets the scenario where the engine has been at high load for an extended duration, which can lead to heat soak.

### Check Engine Light Logic

MS3 also has built-in logic for triggering a check engine light:

> "MS3 has Check Engine Light logic, looks like min/max, and 'fluctuation' detection" — @ggurov

The CEL logic in MS3 appears to operate on two detection modes:
1. **Min/Max threshold** — triggers a CEL if a monitored value goes outside a configured range
2. **Fluctuation detection** — triggers a CEL if a monitored value oscillates or fluctuates beyond expected bounds

This is notable as it provides a factory-style diagnostic indicator from within the MS3 firmware, without requiring external hardware or additional ECU outputs beyond a simple warning light. This is useful for street-driven vehicles where the driver needs an immediate indication of an out-of-range condition without monitoring a laptop display.
