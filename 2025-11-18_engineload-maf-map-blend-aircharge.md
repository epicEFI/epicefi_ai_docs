# engineLoad Calculation: Speed Density vs MAF and the MAP/MAF Blend

*Source: rusEFI Discord, 2025-11-18 | Channel: epicEFI-announcements (1376346219097620492)*
*Contributors: @ggurov, @robb235_00916*

## Summary

In epicEFI/rusEFI, the `engineLoad` value that drives fueling and ignition tables is not a raw sensor reading but a blended, weighted average of two air-charge estimation methods: speed density (using MAP) and mass airflow (MAF). Understanding how the blend works is essential for correctly interpreting and tuning the fuel and ignition tables.

## Details

### Target AFR Control Axis

A user asked whether target AFR is still controlled by MAP vs RPM. The answer is that target AFR tables use `engineLoad` as the load axis, which is the blended value described below — not raw MAP directly.

### How engineLoad Is Computed

The system supports two air-charge estimation models:

- **Speed density model**: engine load is represented by `MAP` (Manifold Absolute Pressure). MAP directly reflects cylinder filling as a percentage of atmospheric pressure.
- **MAF model**: engine load is represented by `AirCharge`, which reaches 100% when the measured air mass matches the cylinder displacement volume.

Both models expose their result under the label "engine load" in the MAF/MAP blended configuration.

The final `engineLoad` value reported to the fuel/ignition tables is:

> **engineLoad = weighted blend of speed density (MAP-based) and MAF AirCharge, governed by the MAP vs MAF blend table**

### MAP vs MAF Blend Table

The blend table determines at each operating point (typically MAP vs RPM) how much weight is given to the speed density result versus the MAF result. At 0% blend the system is pure speed density; at 100% it is pure MAF. Intermediate values linearly interpolate between the two air-charge estimates.

### Practical Implications

- All load-indexed tables (fuel, ignition, AFR target, etc.) see this blended `engineLoad` value, not raw MAP or raw MAF output directly.
- When running pure speed density (blend = 0), `engineLoad` equals MAP-derived load; when running pure MAF (blend = 100%), `engineLoad` equals AirCharge.
- Mixed blend configurations require careful calibration of both the VE/injector characterization and the blend table itself.
