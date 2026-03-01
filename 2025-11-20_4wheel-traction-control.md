# 4-Wheel Traction Control with Per-Wheel Differentiated Response

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov (@ggurov), Tera (@teraflopping)*

## Summary

epicEFI implements a 4-wheel traction control system using individual wheel speed sensor inputs. The system supports differentiated left/right and front/rear responses, allowing asymmetric traction intervention. A closed-loop slip percentage target was proposed as a future enhancement.

## Details

**Implementation status (as of 2025-11-20):**
- 4-wheel traction control is implemented and "begging for testing" (ggurov).
- Requires individual wheel speed sensors on all four corners — supported via the Mega2560 expander's quad VSS inputs or via CAN bus (e.g., factory ABS wheel speed data from the vehicle CAN network).

**Differentiated response:**
- The system supports independent left/right and front/rear traction intervention responses.
- This allows the ECU to apply different levels of torque reduction or ignition retard depending on which wheel(s) are slipping and their position in the vehicle.

**Slip target levels:**
- Multiple traction trim levels are available.
- Amplification of trim response can be adjusted in real-time from the button box.

**Proposed enhancement — closed-loop slip percentage targeting:**
- Tera proposed adding a closed-loop algorithm to target a specific slip percentage (rather than just reacting to detected slip).
- Rationale: approximately 10% wheel slip is considered optimal for maximum longitudinal acceleration (peak of the friction circle / slip ratio curve).
- Further proposal: system could build a memory of optimal slip parameters for different surfaces, temperatures, and boost maps.
- Status: proposed/not yet implemented as of this discussion.

**Input sources:**
- Wheel speed data can come from:
  1. Physical VSS sensors wired directly to the Mega2560 expander (quad VSS inputs).
  2. Factory ABS wheel speed data transmitted on the vehicle CAN bus — epicEFI can read these via a CAN listener/RPC.

**Relation to traction trim UI:**
- Traction trim amplification is adjustable via button box in real-time.
- Multiple named trim levels exist (specific values not disclosed in this discussion).
