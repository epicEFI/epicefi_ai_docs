# VEAL Auto Tune Tips — Cell Percentage Limits, Idle Locking, and Deceleration Tuning

*Source: rusEFI Discord, 2026-03-01 | Channel: 1356732771325968630*
*Contributors: @MykkH, @paleppp, @Joesphan*

## Summary

A cluster of practical tips from experienced tuners covers three related autotune topics: setting VEAL's Max Cell Percentage Change to avoid overshoot, locking idle cells to prevent AFR instability on overrun-to-idle transitions, and tuning out unwanted deceleration pops and bangs by leaning the VE table and zeroing ignition advance in the overrun region.

## Details

### VEAL Max Cell Percentage Change

VEAL (Volumetric Efficiency Auto-Learn in TunerStudio) operates on a fixed update cycle of approximately 12–15 seconds. Because corrections are applied based on data that may be up to 15 seconds old, a large Max Cell Percentage Change can cause the algorithm to overshoot the VE target:

- The system makes a correction based on old data.
- Before the effect of that correction is measurable, the next cycle fires and applies another correction in the same direction.
- This leads to large swings around the target rather than convergence.

Recommended setting: **Max Cell Percentage Change = 2–4%**

At this setting, VEAL makes small incremental adjustments each 12-second window. If the cell is still off-target after the first correction, the next update will apply another 2–4% in the same direction, converging gradually without overshooting. Larger percentage values are tempting for faster tuning but frequently overshoot when data is stale, especially at transient operating points.

### Locking Idle VE Cells

When returning to idle from overrun (engine braking), the transition through the overrun fuel-cut region can cause large AFR excursions. This is made worse by long-tube headers, which have greater exhaust gas volume and slower scavenging — the residual exhaust composition reaching the wideband is not representative of combustion AFR during this transient.

Recommendation: **lock the idle VE cells** in TunerStudio so that VEAL cannot modify them during overrun-to-idle events. Manually set idle VE values to achieve the desired idle AFR, then lock those cells. This prevents autotune from writing incorrect values driven by the overrun transient.

### Reducing Deceleration Pops and Bangs

Unwanted pops and bangs on deceleration are caused by unburned fuel igniting in the exhaust. The two-part fix:

1. **Lean the VE table in the overrun region** (high RPM, low MAP / high vacuum cells): reduce VE values so less fuel is injected during deceleration fuel-cut transitions.
2. **Set ignition advance to 0° in the same overrun cells**: late or zero timing means any fuel that does enter the cylinder burns less aggressively and does not generate exhaust detonation events.

Both changes must be applied to the same cells for the fix to be effective. Leaning VE alone reduces (but may not eliminate) the effect; the ignition change completes the suppression.

For a staged deceleration effect (one or two controlled pops before full fuel cut), the VE and ignition values can be tuned to intermediate values rather than fully zeroed.

## Notes

- The "Change Easiness" parameter in VEAL (also visible in the session discussion) controls how aggressively VEAL responds to AFR error; it is separate from Max Cell Percentage Change. No specific guidance on that value was given in this session.
- The lambda delay table in TunerStudio (project folder, not saved to the ECU) affects how long VEAL waits after a throttle change before making corrections — relevant when tuning in traffic or with frequent throttle inputs.
- Locking idle cells is especially important for engines with large-volume exhaust systems (long-tube headers, equal-length manifolds on high-displacement engines) where exhaust gas dynamics create large transient AFR spikes at the sensor.
