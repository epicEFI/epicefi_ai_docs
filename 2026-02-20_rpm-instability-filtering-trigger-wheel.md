# RPM Instability, Filtering, and Trigger Wheel Configuration

*Source: epicEFI Discord, 2026-02-20 | Channel: 1356732771325968630*
*Contributors: @Alice (@__._.alice._.__), @jeybee (@_jeybee), @Ognjen Galic (@ogalic), @Joesphan (@joesphan)*

## Summary

During a tuning session, erratic RPM readings were observed in logs when significant throttle input was applied. The suggestion to enable RPM filtering was raised as a solution to stabilize readings. Separately, a trigger wheel configuration using twin optical sensors (1-tooth cam and 24-tooth crank) was described for the same engine.

## Details

### Erratic RPM Readings Under Throttle

- Logs showed RPM values that were "all over the place" when Alice gave heavy throttle input.
- Alice noted the instability correlated with aggressive throttle use: "Well I'm giving it throttle a lot too."
- @Ognjen Galic suggested **enabling filtering** in the ECU settings as a remedy: "Have you tried enabling filtering, Alice?"
- Enabling filtering smooths RPM signal processing, reducing the noise in readings caused by mechanical or electrical signal variance under load.

### Trigger Wheel Configuration (Twin Optical Sensor Setup)

- @Joesphan asked about the number of teeth on the trigger wheel.
- @jeybee described the setup: **twin optical sensor configuration**:
  - One sensor: **1 tooth** (cam/sync reference)
  - Second sensor: **24 teeth** (crank position)
- This pattern (1-tooth cam + multi-tooth crank) is a common setup for sequential injection and ignition, providing both cycle synchronization and high-resolution crank position.

### Trigger Timing Offset Drift

- @Hydrocarbon reported a separate but related issue: "My first start would have been fine except the trigger timing keeps needing an extra **20 degrees** adjustment every few starts."
- This recurring +20 degree drift suggests a potential sensor alignment issue, a loose trigger wheel, or a firmware/configuration problem where the trigger offset does not persist correctly across restarts.
- @Hydrocarbon later mentioned the same issue affecting their uaEFI: "I can't seem to figure out why my uaefi keeps needing 20deg pulled off the trigger number each restart."
- Recommended checks: verify crankshaft position sensor (CKP) and camshaft position sensor (CMP) alignment and mounting security; check that trigger offset is being saved correctly in the configuration.
