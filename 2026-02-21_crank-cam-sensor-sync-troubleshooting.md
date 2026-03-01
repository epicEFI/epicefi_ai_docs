# Crank and Cam Sensor Sync Troubleshooting (B-Series Engine)

*Source: rusEFI Discord, 2026-02-21 | Channel: 1356732771325968630*
*Contributors: @Bearded_Bogan, @ggurov, @Joesphan, @offtheband*

## Summary

A user (Bearded_Bogan) performed a first-start attempt on a B-series engine running epicEFI and encountered trigger sync errors. The crank sensor was receiving RPM but had sync errors caused by incorrect edge selection. The cam sensor was not detected at all. Diagnosis and remediation steps were discussed, including flipping the trigger edge, and using the VVT rise counter in logs to verify cam sensor connectivity.

## Details

### Initial Symptoms

- Crank sensor was functional — RPM values were being received.
- Cam sensor showed no input at all.
- Trigger sync errors were present in logs.
- Trigger settings had been sourced from a known-running maxxwcu tune file as a reference baseline.

### Root Cause: Wrong Trigger Edge

The core issue with the crank sync errors was incorrect edge selection on the crank input:

- `ggurov` advised: **"flip the edge"** — the crank trigger was configured to capture on the wrong edge (rising vs. falling).
- `Joesphan` confirmed: **"your cam is flaky not crank"** initially, but `ggurov` countered that errors actually originate at the crank, with the cause being **"either too many teeth or not enough teeth"** — which is a symptom of incorrect edge triggering causing the decoder to miscount teeth.
- Recommended action: **"flip crank first, log again"** — change the crank edge setting and re-capture a log before attempting to resolve the cam issue.
- Bearded_Bogan's follow-up plan: **"set crank to rising and see if it helps"**.

### Cam Sensor: Not Detected

- No cam sensor input was visible. The second cam signal also could not be located.
- Recommendation: resolve crank sync errors first before tackling the cam input — **"one thing at a time"**.

### Verifying Cam Sensor Connectivity via Logs

A separate user (offtheband) asked how to confirm a cam sensor is wired correctly, similar to the feedback available for TPS and ETB:

- `ggurov` explained: **a number will increment in the logs every tooth** — look for a **"rise"** counter in the log (e.g., the VVT rise counter).
- Specifically, look for something labelled **"rise"** in the log channels — this increments each time the cam sensor fires.
- This provides a wiring-sanity check without needing to fully start the engine.

### Diagnostic Workflow Summary

1. Check logs for trigger sync errors.
2. If sync errors are present at the crank: flip the crank trigger edge (rising/falling).
3. Re-capture a crank log and verify RPM reads cleanly without sync errors.
4. Only then investigate cam sensor issues.
5. To verify cam sensor wiring: monitor the VVT "rise" counter in logs — it should increment each time the cam tooth passes.
