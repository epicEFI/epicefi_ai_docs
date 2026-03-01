# Dual MAP Sensor Speed Density for Boosted Engines

*Source: rusEFI Discord, 2025-11-19 | Channel: 1401895238481744052*
*Contributors: @fast335xi, @mykkh, @joesphan, @ggurov*

## Summary

A user asked about implementing a modified speed density load calculation using two MAP sensors simultaneously: a 1 bar (absolute) manifold sensor for naturally-aspirated/vacuum operation, and a 4 bar sensor on the charge pipe for boosted operation. The rationale is improved resolution in the idle/vacuum range where a 4 bar sensor alone would have poor accuracy. The feature was confirmed feasible.

## Details

### Proposed Configuration

- **Sensor 1**: 1 bar MAP sensor in the intake manifold — active from vacuum up to ~1 bar absolute (atmospheric)
- **Sensor 2**: 4 bar MAP sensor on the charge pipe (post-compressor, pre-throttle) — active above 1 bar absolute (boost conditions)
- The ECU switches between sensors at the 1 bar threshold

### Rationale

- A single 4 bar sensor across the full operating range has poor resolution at idle/light load: idle manifold pressures (~0.3–0.5 bar absolute) would occupy only the bottom quarter of the sensor's range
- The 1 bar sensor provides 4x better resolution in the vacuum/idle range compared to a 4 bar sensor
- This is primarily an **accuracy improvement**, not a fundamentally different load sensing strategy — @fast335xi confirmed after reviewing BMW MSD80 ECU code that throttle mass flow (TMF) in that system is a calculated derived value used mainly for limits, not the primary load input
- @mykkh asked if a single 4 bar manifold sensor would suffice — the answer is that the charge pipe placement + manifold placement together capture pressure drop across the throttle body, which a single manifold sensor cannot measure; however, the main benefit here is the resolution gain at low load

### Feasibility

- @ggurov confirmed the approach as feasible ("sure")
- Implementation would require configuring rusEFI to treat the two sensor inputs as a unified load signal with a switchover point at 1 bar

### Related Speculation

- @joesphan speculated the user may also be interested in throttle mass flow (TMF) calculation, which requires knowing both upstream (charge pipe) and downstream (manifold) pressure simultaneously
