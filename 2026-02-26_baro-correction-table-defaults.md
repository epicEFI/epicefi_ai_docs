# Barometric Pressure Correction Table: Default Values and Behavior

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @ggurov, @jeybee, @SanGawku, @ZeeKay, @kitaji_SR20*

## Summary

The barometric pressure correction table (`baroCorrTable`) in rusEFI defaults to all 1.0 (unity), meaning no correction is applied by default. The firmware explicitly acknowledges that a "reasonable default" cannot be recommended for this table because the appropriate values are highly site-specific. Users without a dedicated barometric pressure sensor should leave all values at 1.0 unless they are using MAP-at-startup baro sampling or a dedicated baro sensor input.

## Details

### Default Firmware Behavior

The firmware initializes the baro correction table to unity (1.0) across all cells. The relevant source code comment:

```c
// Default baro table is all 1.0, we can't recommend a reasonable default here
setTable(config->baroCorrTable, 1);
```

This means an untouched baro correction table produces no fuel correction. The table will not automatically zero out or reset — any values a user enters accumulate until manually cleared.

### Baro Correction Table Structure

The baro correction table uses RPM-based breakpoints on one axis and barometric pressure on the other. @ggurov described the table as "poor man's EMAP" — a way to apply corrections based on atmospheric pressure changes as a proxy for charge density variation.

### Behavior Without a Baro Sensor

If neither a dedicated baro sensor input nor "grab baro from MAP at startup" is configured:
- The ECU uses the firmware's "default baro" value (effectively `STD_ATMOSPHERE`, approximately 100 kPa) as its assumed atmospheric pressure
- The baro correction table is still active, so non-unity values in the table will apply fuel corrections based on this fixed assumed pressure
- Unless you have a baro sensor or use MAP-at-startup sampling, leaving all baro correction table cells at 1.0 is the correct approach to avoid unintended fuel corrections

### MAP-at-Startup Baro Sampling

rusEFI supports reading MAP sensor voltage at key-on (before cranking, when the engine is not running) to estimate ambient barometric pressure. This is a common technique used by many ECUs from the 1990s onward. If this feature is enabled, the baro correction table will use the sampled pressure rather than the fixed default.

### MAP Sensor Fallback

The firmware has no automatic fallback strategy if the MAP sensor fails during operation — there is no limp-home substitute source for MAP data.

### Practical Guidance

- If running without a baro sensor and without MAP-at-startup: set all baro table cells to 1.0
- If at high elevation or racing on tracks with significant elevation change: consider adding a baro sensor or enabling MAP-at-startup
- For typical road use at a fixed altitude, baro correction has minimal effect unless elevation changes are extreme (e.g., Pike's Peak-style events)
- Japan's speed tracks with extreme elevation changes have been reported to affect fueling noticeably without baro compensation
