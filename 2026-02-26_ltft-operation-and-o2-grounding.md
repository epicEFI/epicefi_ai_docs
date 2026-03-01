# Long Term Fuel Trim (LTFT): Operation, Prerequisites, and O2 Sensor Grounding

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @kostasl, @Joshan_Lu (@joshseek_30236_07984), @ggurov*

## Summary

LTFT in rusEFI operates continuously rather than requiring manual activation — it accumulates corrections into its table automatically as the engine runs. STFT must be dialed in well before LTFT can work correctly, since LTFT builds on STFT data. A key hardware prerequisite is grounding the wideband O2 sensor to the ECU chassis ground rather than to a remote body/chassis point, as improper grounding introduces noise that degrades closed-loop fueling accuracy.

## Details

### How LTFT Accumulates

- LTFT does **not** zero out automatically — values accumulate in the table over time and persist until the user manually clears them
- STFT corrections "tick over" into LTFT as they accumulate; LTFT is not a separately triggered process
- If STFT is near zero consistently (i.e., the base fuel map is well-tuned), LTFT will also remain near zero and is essentially unnecessary in that case
- The "apply correction" setting in the LTFT menu does not gate when corrections are written to the table — corrections are applied continuously while the feature is enabled

### LTFT Prerequisites

1. **STFT must be stable and well-behaved first.** Attempting to use LTFT with a poorly tuned base map or erratic STFT leads to unpredictable accumulation in the LTFT table.
2. **Wideband O2 sensor must be properly grounded** (see below).

### Use Cases for LTFT

LTFT is most useful for compensating for slow, long-term changes in fueling conditions:
- Fuel quality variation (e.g., water contamination)
- Atmospheric pressure changes
- Air filter becoming restrictive
- Component wear over time

### Applying an Existing Trim Table

If a trim table has already been populated and the user wants to activate it: enable the LTFT feature in the settings. The table values are applied as long as the feature is active. There is no separate "apply" button to push — the table is live once enabled.

### O2 Sensor Grounding (Critical)

**Ground the wideband O2 sensor signal ground directly to the ECU, not to a remote chassis or body ground point.**

- @Joshan_Lu reported grounding their O2 sensor to "a random spot on the steering column" which caused issues with STFT behavior
- The O2 sensor ground must share the ECU's ground reference to avoid ground offset voltage corrupting the lambda signal
- A poor O2 ground manifests as erratic or offset STFT readings, which then corrupt LTFT accumulation

### General Closed-Loop Tuning Philosophy

- A perfectly tuned base VE table requires minimal STFT and renders LTFT unnecessary
- LTFT is a self-correction tool for real-world variability, not a substitute for initial tuning
- Users with good base maps will see minimal LTFT correction values accumulating over time
