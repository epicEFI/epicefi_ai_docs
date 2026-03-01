# Cam Sensor Sync Loss Above 3000–3500 RPM: VR Conditioner Saturation and Workarounds

*Source: rusEFI Discord, 2025-11-24 | Channel: build-help (1405892994506424421)*
*Contributors: Joshan Lu (@joshseek_30236_07984), Joesphan (@joesphan)*

## Summary

Joshan Lu reported major synchronization loss on his epicEFI-equipped build above approximately 3000–3500 RPM. The likely cause was the cam sensor signal saturating or overdriving the VR conditioner. Two approaches were discussed: a short-term workaround (disabling cam resync above a threshold RPM) and a hardware fix (resistor voltage divider in series with the cam sensor to reduce the signal amplitude before clamping). The workaround allowed the build to run reliably to redline at ~10 psi boost.

## Details

### Symptom

- Major sync loss occurring above **3000–3500 RPM**.
- Engine could not rev past 3000 RPM before the workaround, because the sync loss triggered a safety response.
- The sync loss manifested through the cam sync / resync counter — the resync counter was not incrementing as expected when the issue was active.
- Joesphan's log review confirmed: *"duty max at 6776 rpm"* for the injector and the log looked good otherwise; the cam sync issue was the primary obstacle.

### Root Cause (Suspected)

The cam sensor output voltage increases with RPM on many VR (variable reluctance) sensors. At higher engine speeds, the signal amplitude can exceed the input range of the VR conditioner, causing it to saturate or otherwise misinterpret the cam pulses. Joshan Lu described it as: *"I think it skitzes the VR conditioner out."*

### Workaround — Disable Cam Sync Above 2500 RPM

Joshan Lu's working solution: disable cam sync/resync above approximately **2500 RPM**.

- In epicEFI, there is a setting to gate cam-based resynchronization below a configurable RPM threshold.
- With cam sync disabled above 2500 RPM, the ECU relies on the initial sync acquired at lower RPM and does not attempt to re-sync using the cam signal at high speed.
- Result: engine ran cleanly to redline and boosted to ~10–11.5 psi without sync events.
- Trade-off: if sync is lost above 2500 RPM for any other reason (e.g., crank signal noise), the ECU cannot recover it without dropping below the threshold.

**Joshan Lu's note:** *"But turning off the resync thing is fine once it goes past 2500 it doesn't lose sync or anything."*

### Hardware Fix — Resistor Voltage Divider on Cam Sensor Input

The proposed permanent fix is to add a **resistor in series with the cam sensor signal line** to divide the voltage down to a level the VR conditioner can clamp properly at all RPM, without reducing the signal so much that cranking speed detection is compromised.

Joshan Lu: *"I might just put in a resistor in line with the cam and divide the voltage down enough to clamp it without cranking problems."*

Key considerations:
- The resistor forms a voltage divider with the conditioner's input impedance.
- The division ratio must be chosen so the signal is reduced enough to prevent saturation at high RPM, while remaining large enough for the conditioner to detect zero-crossings reliably at cranking speed (low RPM = low amplitude VR output).
- This is a common fix for high-output VR cam sensors on aftermarket ECU installations. A series resistor in the range of **1–10 kΩ** is typical, but the exact value depends on sensor output impedance and conditioner input characteristics.
- After fitting the resistor, re-verify cranking sync acquisition before testing at high RPM.

### Log Analysis Notes

Joesphan reviewed logs from the session:
- Injector hit maximum duty cycle at 6776 RPM — indicating the tune is close to injector limits at that RPM.
- No O2 sensor was plumbed in at time of testing; Joesphan flagged this as something to address.
- Outside of the cam sync issue, the log was described as looking good.
