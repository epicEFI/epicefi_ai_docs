# Trigger Logger Error Interpretation and uaEFI 9007 Error Code

*Source: epicEFI Discord, 2026-02-27 | Channel: 1356732771325968630*
*Contributors: @Robb235, @ggurov*

## Summary

Two related trigger diagnostics topics came up in the same channel. First, Robb235 confirmed how to read the trigger logger: a red line that jumps high means a trigger error has occurred. Second, while helping another user set up uaEFI on epicEFI firmware, a 9007 error code was encountered. The tooth logger for that setup showed 34 teeth followed by 2 missing teeth (consistent with a 36-2 wheel), which appeared healthy.

## Details

### Trigger Logger: Reading the Red Line

- The trigger logger in rusEFI/epicEFI displays trigger health in real time.
- **Red line behavior**:
  - Red line sitting at the **bottom** → no errors, normal operation.
  - Red line **jumping to the high end** → indicates a **trigger error** (sync loss or unexpected tooth pattern).
- Confirmed by @ggurov: "yes, red line means error."

### uaEFI Setup on epicEFI Firmware: Error Code 9007

- A user setting up **uaEFI hardware on epicEFI firmware** encountered error code **9007**.
- The tooth logger showed the trigger pattern as: **34 teeth + 2 missing** (36-2 wheel).
- The tooth pattern itself appeared correct to Robb235, suggesting the 9007 error may be a configuration or firmware compatibility issue rather than a wiring/sensor problem.

### Interpretation of 36-2 Wheel in Trigger Logger

- A 36-2 crankshaft trigger wheel has 36 evenly-spaced tooth positions with 2 adjacent teeth removed (the "missing tooth" gap).
- Expected tooth logger output: 34 present teeth, then a gap of 2 missing teeth per revolution.
- If the tooth logger shows this pattern correctly, the physical sensor wiring and reluctor wheel are working as intended.
- Error code 9007 in this context likely points to a **configuration mismatch** (e.g., trigger type, tooth count, or sync settings not matching the connected hardware profile).

### Recommended Diagnostic Steps for 9007

1. Verify the trigger type selection in the ECU configuration matches the actual wheel (36-2).
2. Check that the number of teeth and missing tooth count are set correctly.
3. Confirm the cam sync sensor type and tooth count are configured to match the installed hardware.
4. Review firmware version compatibility between uaEFI hardware and epicEFI firmware.
