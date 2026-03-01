# ETB H-Bridge Troubleshooting: No Motor Movement, Fuse, and Wiring Checks

*Source: epicEFI Discord, 2026-02-25 | Channel: 1356732771325968630*
*Contributors: @Hydrocarbon, @ggurov*

## Summary

A user with a uaEFI board and a 6-wire GM electronic throttle body (ETB) could not get the throttle motor to move during autocalibration. The TPS feedback worked correctly, but the motor would not respond and the system immediately threw an error. Systematic diagnosis revealed a blown/faulty solder joint on the carrier board's 5A fuse, which was preventing power from reaching the H-bridge outputs. Key diagnostic steps covered H-bridge configuration verification, 12V power supply checks, and continuity testing of the H-bridge output pins through to the wiring harness.

## Details

### Setup

- **Board**: uaEFI (running stock rusEFI firmware as shipped)
- **Throttle body**: 6-wire GM ETB
- **Supply voltage**: 12.34V (measured), BATV showing 12.3V in TunerStudio

### Symptom

- ETB motor does not move at all during autocalibration.
- No jiggle or partial movement -- motor is completely unresponsive.
- System throws an error immediately on autocalibration attempt.
- Three different ETBs were tried with the same result, ruling out a faulty throttle body.
- TPS signal works correctly: disconnecting MOT+/MOT- and manually moving the throttle blade shows correct voltage sweep on TPS1 and TPS2.
- Applying 12V directly to MOT+ causes the throttle to open fully (motor confirmed functional).

### Diagnostic Steps

#### 1. Verify ETB Motor Wiring

Confirm MOT+ and MOT- pin assignments from the board pinout documentation. To identify polarity:
- Apply 12V to one motor wire with the other grounded.
- If the throttle opens, that is MOT+.
- If it slams shut, polarity is reversed (that is the MOT- wire).

#### 2. Verify 12V Power to the Board

The H-bridge requires its own 12V supply. The uaEFI/Honda-style boards typically require:
- 12V to VIGN (ignition-switched supply)
- 12V to the dedicated 12V power pin (fused)

Both 12V connections must be present for the H-bridge to operate. Confirm BATV reads ~12V in TunerStudio.

#### 3. Verify H-Bridge Configuration in Firmware

Check that the H-bridge is configured for the correct output pins matching the physical wiring:
- @ggurov: "what board is this? and do you have the h-bridge configured right?"
- The uaEFI board's default H-bridge configuration should match the physical ETB connector ports -- verify this is not misconfigured.

#### 4. Check Continuity from H-Bridge Output Pins to Harness

With the board powered off, use a multimeter to check continuity from the H-bridge output pins on the PCB through to the ETB connector in the wiring harness. Lack of continuity indicates a break in the circuit.

#### Root Cause Found: Bad Solder Joint at Fuse

During continuity testing, @Hydrocarbon discovered that the carrier board's **5A fuse** had a bad solder joint:
- Continuity checked through the fuse measured as good (the fuse element itself was intact).
- However, when the board was powered and under load, no voltage appeared on the output side of the fuse.
- This indicated a cold/cracked solder joint at the fuse pad -- resistance appeared low with a meter but could not pass current properly when energized.

**Resolution**: Reflow or resolder the fuse on the carrier board.

### Key Lessons

- Always verify both 12V connections to the board (VIGN + power pin) -- missing one can cause the H-bridge to fail silently.
- A fuse that measures continuity with a multimeter can still have a bad solder joint that fails under load. Check voltage on both sides of the fuse while powered.
- TPS working correctly while the motor does not move strongly suggests the H-bridge power path (not the signal/sensor path) is the problem.
- If three different ETBs all fail identically, the problem is in the ECU/board, not the throttle bodies.
