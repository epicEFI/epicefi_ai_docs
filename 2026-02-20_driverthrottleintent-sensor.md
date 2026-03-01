# DriverThrottleIntent Sensor: Combined TPS and PPS Input

*Source: rusEFI Discord, 2026-02-20 | Channel: 1401895238481744052*
*Contributors: @ggurov*

## Summary

rusEFI/epicEFI exposes a virtual sensor called `DriverThrottleIntent` that combines data from both the Throttle Position Sensor (TPS) and the Pedal Position Sensor (PPS). Its behavior adapts automatically based on whether the vehicle uses an electronic throttle body (ETB) or a cable-actuated throttle setup.

## Details

### What is DriverThrottleIntent?

`DriverThrottleIntent` is a derived/virtual sensor value representing the driver's intended throttle demand. It is not a raw hardware sensor input but rather a software-computed composite:

- **ETB (drive-by-wire) setups**: The value is derived from the **PPS (pedal position sensor)**, which directly measures driver foot pressure on the accelerator pedal.
- **Cable throttle setups**: The value is derived from the **TPS (throttle position sensor)**, which measures the actual throttle plate angle.

### Usage

`DriverThrottleIntent` is used internally by the ECU for features that need a consistent representation of driver demand regardless of throttle actuator type. This allows fuel, spark, and other control strategies to reference a single unified signal rather than branching logic for ETB vs. cable configurations.

### Configuration

The ECU automatically selects the correct source (TPS or PPS) based on whether ETB is configured in the tune. No manual override of the source is typically required.
