# Sharing VR Sensors Between Standalone ECU and OEM ECU

*Source: rusEFI Discord, 2026-02-24 | Channel: 1356732771325968630*
*Contributors: @tren, @ggurov, @Ognjen Galic*

## Summary

When running a standalone ECU alongside an OEM ECU (piggyback or semi-replace configuration), it is possible to share VR (variable reluctance) sensors such as crank sensors between both ECUs. The key constraint is that the standalone ECU must tap into the sensor signal rather than cutting the wire, so the OEM ECU continues to receive its signal. However, the conditioner hardware on the standalone ECU can interfere with the OEM ECU's ability to read the sensor, requiring inline resistors or careful conditioner selection.

## Details

### Tapping vs. Cutting

When sharing sensors with the OEM ECU, do not cut the sensor wires. Instead, tap into them so both ECUs receive the signal simultaneously. The standalone ECU should be connected in parallel, leaving the OEM ECU's path intact.

### VR Conditioner Interference

The user (@tren) was experiencing significant noise from the VR sensor when using a board with a built-in MAX9926 dual VR conditioner. The MAX9926 is a selectable VR or Hall input conditioner used in some ECU boards (e.g., the Nick Hay Pro ECU).

When connected without the tap to the factory ECU, noise behavior differed. The MAX9926 conditioner itself was contributing to signal conflicts.

### Load Resistor Issue (MaxxECU Example)

@Ognjen Galic reported a specific real-world case: the MaxxECU includes a **6.8 kOhm load resistor** on the VR input circuit. When the MaxxECU is connected in parallel with the OEM ECU on the crank sensor, this load resistor can prevent the OEM ECU from properly detecting the crank sensor signal.

- **Root cause**: The 6.8 kOhm pull-down/load resistor in the MaxxECU's conditioner circuit loads down the signal line sufficiently to degrade or eliminate the OEM ECU's sensor reading.
- **General principle**: Whether a VR sensor can be shared depends on the hardware conditioner design in the standalone ECU.

### Workaround: Inline Resistors

If the standalone ECU's conditioner is loading the signal and preventing the OEM ECU from reading it, adding resistors inline between the shared tap point and the standalone ECU can increase the effective impedance seen by the standalone ECU's input, reducing its loading effect on the signal line. This preserves signal integrity for the OEM ECU.

- Suggested by @ggurov: "you can try some resistors inline to the standalone ecu"

### Summary of Conditions for Successful Sensor Sharing

1. Tap into sensor wires; never cut them when OEM ECU must still function.
2. Check whether the standalone ECU's VR conditioner has a significant load resistor (e.g., 6.8 kOhm in MaxxECU).
3. If sharing fails, try adding inline resistors between the tap point and the standalone ECU input to reduce loading.
4. Some conditioners (e.g., MAX9926-based) may be selectable for VR or Hall mode -- ensure the correct mode is selected for the sensor type.
