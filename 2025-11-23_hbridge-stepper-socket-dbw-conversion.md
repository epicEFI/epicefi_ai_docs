# H-Bridge Drop-In for Stepper Socket: Drive-By-Wire Conversion

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov)*

## Summary

A practical approach to converting a stepper-motor idle air control (IAC) valve setup to electronic throttle body (ETB/drive-by-wire) control: install an H-bridge driver module in the existing stepper socket and reuse the original stepper wiring harness. A large filter capacitor is required to stabilize the H-bridge supply rail.

## Details

**The conversion approach:**

If an engine originally used a stepper motor IAC valve (common on older vehicles), the existing stepper driver socket and four-wire harness can be repurposed for ETB control. An H-bridge module sized for the DBW motor load is physically installed in the stepper socket footprint, and the same wiring is used to drive the H-bridge.

ggurov's summary: "put h-bridge where stepper module was, use same wiring."

The ECU side is configured to use the stepper socket outputs as an H-bridge (two half-bridges, two channels each) rather than as a stepper sequence. In rusEFI/epicEFI this maps to the drive-by-wire ETB configuration targeting the stepper output pins.

**Capacitor requirement:**

Without a bulk capacitor on the H-bridge supply rail, the module runs extremely hot and the motor drive is unstable. A large electrolytic capacitor on the supply node is essential for stable operation. The capacitor smooths the current spikes from PWM switching and prevents voltage droop during motor energization.

**Hardware context (ggurov's implementation):**

- H-bridge module for DBW motor, installed in the stepper socket
- STM32H743 (144-pin LQFP) as the primary microcontroller, mounted on an Mega2560-footprint adapter board
- 6-layer PCB layout
- UEGO/wideband O2 is handled off-board (external controller, e.g., 14point7), not integrated on the main board

**Practical note:**

This approach is described as a "super lazy" retrofit — a fast way to add ETB control to a car that originally had stepper idle, without rewiring. It is functional but the thermal behavior without the capacitor makes that component non-optional.
