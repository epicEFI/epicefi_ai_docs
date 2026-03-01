# OBD1 Honda Trigger Resync and Stall Troubleshooting

*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @ggurov, @sheckta_1593, @joesphan, @joshseek_30236_07984*

## Summary

A user running an OBD1 Honda with rusEFI (on an Andrey UAEFI OBD1 all-in-one board) experienced intermittent stalls diagnosed as trigger resync events. The resync counter incrementing indicates the decoder detected an unexpected cam tooth, causing the engine phase to drop out. The root cause was traced to electrical noise — likely related to the internal distributor coil — causing the cam tooth to appear too early in the timing window.

## Details

### Symptoms

- Engine stalls intermittently during steady-state driving
- Resync counter increases at the point of stall (visible in TunerStudio live data)
- Error code 6675 present: VVT cam sync error
- When the resync counter is NOT incrementing, the engine phase signal (0–720 degree value, shown in yellow in the composite logger) tracks cleanly

### Diagnosis from Composite Logger

- The yellow channel in the composite logger shows engine phase (0–720 degree cycle)
- Resync events appear as breaks in the regular pattern — the phase value jumps out of sequence
- At the stall event, an extra cam tooth was being seen by the decoder
- The cam tooth was arriving earlier than expected — described as arriving before the expected "offset + TDC" marker
- At RPM there is a known cam timing offset table to compensate for belt/chain stretch; Honda stock ECUs use a similar correction

### Electrical Noise Hypothesis

- The coil being physically inside the distributor is suspected as the noise source
- Noise causes the VR conditioner output to generate a spurious edge at the cam sensor
- This is described as a "compounding effect" — a small disturbance causes a ripple that pushes the sync state to the edge of failure, and one more disturbance causes a complete sync loss
- The Spanish-language community channel noted this is a known occurrence on OBD1 Honda setups; Hondata reportedly uses a correction value for the sync offset

### VR Polarity Check (Threads 27)

- @joesphan raised the possibility that VR+ and VR- connections could be swapped
- A swapped VR polarity causes the VR conditioner to produce a "jumpy signal in the time domain" — the edge moves, making the decoded tooth position jitter
- The effect can be intermittent rather than constant: the signal may be right on the edge of the conditioner's hysteresis threshold, causing it to sometimes trigger correctly and sometimes not
- @joshseek noted the edge itself can shift position, leaving the engine "on the cliff" and vulnerable to sync loss

### Attempted Workaround

- Setting "maximum cam/VVT sync RPM" to 500 RPM was suggested: the ECU ignores cam sync above this RPM, relying on crank-only sync for higher RPM running
- User (@sheckta_1593) applied this setting and was testing at the time of the conversation
- Note: this workaround may not apply to engines without a missing-tooth crank wheel

### Hardware Context

- ECU: Andrey UAEFI OBD1 all-in-one board
- Engine: OBD1 Honda
- Issue: internal distributor coil generating EMI coupling into cam sensor signal path
- Recommended long-term fix: external coils (COP conversion) to eliminate intra-distributor noise
