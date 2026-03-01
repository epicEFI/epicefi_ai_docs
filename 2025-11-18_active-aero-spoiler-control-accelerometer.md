# Active Aero Spoiler Control and epicEFI Onboard Accelerometer

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat (1356732771325968630)*
*Contributors: @Alice, @Joesphan*

## Summary

Alice asked about alternative trigger strategies for controlling an active aero spoiler beyond the basic brake-activation approach. The conversation led to discussion of the epicEFI onboard accelerometer and what motion data it can provide, including the distinction between linear acceleration and lateral (cornering) G-force, and whether the system can detect deceleration.

## Details

### Active Aero Spoiler Control Triggers

Alice was exploring alternatives to using the braking signal as the sole trigger for deploying an active rear spoiler. She was specifically interested in easily-achievable options:

> "Curious if you guys can think of any clever ways to manually hook up my active aero spoiler other than to just flip up when braking? Ofc I can think of other applications, but ones that would be easily achievable" — @Alice

Potential triggers that can be derived from epicEFI data include:
- **Brake activation** (the baseline approach)
- **Vehicle speed threshold** — deploy above a configurable speed
- **Throttle position** — retract on heavy acceleration, deploy on lift-off
- **Deceleration detection** — negative acceleration as detected by the accelerometer or RPM rate of change
- **Cornering G-force** — deploy under high lateral load

### epicEFI Onboard Accelerometer

Joesphan confirmed that the epicEFI hardware includes an onboard accelerometer:

> "epic has onboard accel" — @Joesphan

Alice clarified an important distinction about what "acceleration" means in context:

> "Yeah, but like cornering g force increases but not acceleration. Maybe by acceleration you mean like the physics definition not the car definition tho" — @Alice

This reflects the standard ambiguity:
- **Physics definition**: acceleration is any change in velocity vector, including lateral (cornering) forces.
- **Car definition**: "acceleration" typically means forward linear acceleration (the car speeding up).

The epicEFI accelerometer measures all axes of acceleration in the physics sense, so both longitudinal and lateral G-forces are available.

### Deceleration Detection

Alice asked whether the system can detect negative acceleration (deceleration):

> "Can it tell if acceleration is negative aka deceleration" — @Alice

With an onboard accelerometer, negative longitudinal acceleration (braking/engine braking) is directly measurable as a negative value on the longitudinal axis. This is distinct from, but complementary to, using RPM rate-of-change for deceleration detection.

### Accelerometer Output Resolution

Alice also asked about the output resolution of the accelerometer:

> "Does it only output values of 2, 4, 8, and 16?" — @Alice

This corresponds to standard accelerometer full-scale range settings (±2g, ±4g, ±8g, ±16g) common on MEMS accelerometers such as the LIS2DH12 or MPU-6050. These are configurable gain/range settings, not discrete output values — within any selected range the output is a continuous (ADC-resolution) measurement.
