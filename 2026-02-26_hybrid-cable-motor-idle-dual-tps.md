# Hybrid Cable/Motor Idle Control with Dual TPS Configuration

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @jeybee, @Ognjen Galic, @tmbryhn, @ggurov*

## Summary

This thread documents a hybrid throttle body configuration where a mechanical cable and a DC motor (H-bridge driven) both act on the throttle blade, using two separate Throttle Position Sensors (TPS). The firmware uses the ETB (electronic throttle body) routine to implement idle speed control via the DC motor while the cable handles driver demand. Current draw is minimal (2-3 A max), and the FETs involved are SOT-223 N-channel devices with very low drain-source impedance.

## Details

### Mechanical Architecture

- The throttle body has two independent actuators:
  1. A **mechanical cable** connected to the accelerator pedal — moves the entire throttle assembly.
  2. A **DC motor (H-bridge driven)** — moves the throttle blade independently of the cable, used for idle control.
- The cable physically moves the whole throttle body assembly; the motor trims the blade angle on top of that.
- The throttle body only allows mechanical opening to approximately **15% maximum** — this is a physical hardware limitation of this design.
- At idle, the **cable TPS does not move** (cable is slack/neutral); only the idle TPS (blade angle sensor) changes.

### Dual TPS Role

| TPS | Measures | Used For |
|-----|----------|----------|
| TPS 1 (Idle TPS) | Actual throttle blade angle (DC motor position) | ETB closed-loop idle control |
| TPS 2 (Cable TPS) | Pedal/cable movement | Driver demand |

- In ETB mode, the firmware sees TPS changes and uses them; the idle TPS serves as the ETB position feedback.
- Implementation: use the **ETB routine** and substitute the idle setpoint for the conventional pedal setpoint during idle conditions.

### Determining Idle Max Authority Range

- Set DC motor duty cycle to **100%** and read the resulting TPS percentage — this is the idle max authority limit.
- This calibration step identifies how far the DC motor can move the blade from its resting position.

### H-Bridge and FET Details

- The H-bridge drives the idle control DC motor only; it is **not required** to run the engine.
- FET type: **SOT-223 package**, N-channel, very low drain-source on-resistance (Rds(on)).
- Current requirement: **2-3 A maximum** — thermally trivial under normal operation.
- A **SOIC-8 gate driver** IC works successfully to drive these FETs.
- Caution: SOT-223 packages require careful attention to power supply and thermal management despite the low current.

### DBW Full Idle Alternative

- An alternative implementation runs the idle portion as **full DBW** (Drive-By-Wire), eliminating the hybrid cable/motor complexity entirely. This has been demonstrated successfully by at least one community member.
