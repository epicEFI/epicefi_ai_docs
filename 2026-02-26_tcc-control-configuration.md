# TCC Control Configuration in epicEFI TCU: Load, TPS, and Speed Parameters

*Source: epicEFI Discord, 2026-02-26 | Channel: 1470466409602744541*
*Contributors: @robb235_00916, @ggurov, @sangawku, @nishirams*

## Summary

Extended discussion from a real-world 2-hour road test of the epicEFI Transmission Control Unit (TCU), focusing on Torque Converter Clutch (TCC) lock/unlock behavior. The conversation covers how load, TPS, and vehicle speed interact to control TCC engagement, configuration workarounds for disabling load-based gating, suggestions for new TPS-based parameters, and how the external TCU can receive engine data via CAN bus.

## Details

### TCC Solenoid Assignment Issue

User @Robb235 discovered that TCC solenoids had become unassigned in the configuration at some point. This caused TCC control to stop working. The fix was to reassign the TCC solenoids in the setup.

### PWM Compatibility for TCC Solenoids

Robb235 raised the question of how to determine whether a TCC solenoid is compatible with PWM control. The general guidance is:
- Check the solenoid's datasheet for PWM compatibility specifications.
- Contact the manufacturer if the datasheet does not include this information.
- PWM-incompatible solenoids may be damaged or behave erratically when driven with a PWM signal.

### Load Parameter: MAP vs. TPS

The "load" parameter used for TCC lock/unlock conditions was questioned. The load source in the TCC control table references **MAP** (Manifold Absolute Pressure), not TPS.

Robb235 felt that TPS was a more appropriate load input for his use case, and requested that the load source be made **user-selectable** (TPS or MAP). After successfully engaging TCC, he confirmed this request: the load input should be selectable by the user.

### Disabling Load-Based TCC Gating (Workaround)

To effectively **remove load from the TCC lock/unlock equation** and rely only on speed and TPS:
- Set **Min Load = 0**
- Set **Max Load = 600** (or a high value such as 999 to completely disable the upper bound)

With Min Load = 0 and Max Load = 600, the load window is wide enough to always pass, leaving **vehicle speed** and **min/max TPS** as the only active conditions governing TCC lock/unlock.

If the intent is to completely bypass load: set Min Load = 0 and Max Load = 999.

### TPS-Based TCC Lock/Unlock: Feature Requests

Two feature suggestions were proposed for a future "Min TPS for TCC Lockup" setting:

1. **"Min TPS for TCC Lockup"** — A dedicated setting that could live in the TCC Lock/Unlock menu (not inside the speed-specific menu). This would define the minimum throttle percentage required for TCC to lock.

2. **Speed-dependent TPS unlock threshold** — Robb235 described the need for a 2D relationship: at higher speeds (e.g., 80 mph in 4th gear), the TCC should unlock only at a higher TPS %, while at lower speeds (e.g., 50 mph), it should unlock at a lower TPS %. A simple flat "Min TPS" threshold does not fully address this; a TPS vs. speed lookup table would be more precise.

### Road Test Feedback (2-Hour Trip)

After a 2-hour test drive using the epicEFI TCU:
- Overall TCC engagement worked, but improvements to control logic are needed.
- Key feedback centered on making TCC lock/unlock more responsive and condition-aware.

### CAN Bus: Sending Engine Data to External TCU

EpicEFI can broadcast engine sensor data over CAN bus to external TCU hardware (e.g., ESP32, Arduino, Mega 2560):
- **TPS** (Throttle Position Sensor)
- **RPM**
- **MAP** (Manifold Absolute Pressure)
- **VSS** (Vehicle Speed Sensor)

@SanGawku confirmed this is supported. @Robb235 had been experimenting with using an **Arduino Mega 2560 as a standalone TCU**, receiving TPS, RPM, and VSS over CAN from the epicEFI ECU.

External TCUs such as **turbolamik** can pick up TPS and load data from the CAN stream to calculate engine torque. RPM and boost pressure are additional inputs used in this calculation.

### Gear Detection via RPM Drop/Spike

@SanGawku suggested a technique for detecting gear change completion in an external TCU:
- Monitor for an **RPM drop or spike shortly after commanding a shift**.
- This RPM transient indicates that the gear has engaged and the shift is complete.
- This method avoids relying on a gear position sensor and works with basic RPM monitoring already present on CAN.

### Torque Converter Duty Signal via ECU Request

The ECU can return a **torque converter duty** value on demand. This is a numeric value tied to the current duty cycle being commanded on the torque converter solenoid, and can be used by external devices to monitor TCC state.
