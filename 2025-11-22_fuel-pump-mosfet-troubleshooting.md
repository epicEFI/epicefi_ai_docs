# Fuel Pump Output Troubleshooting and MOSFET Probing

*Source: rusEFI Discord, 2025-11-22 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Tera (@teraflopping), Joesphan (@joesphan)*

## Summary

Discussion of integrating fuel pump solenoid control into the ECU via a FET output, diagnosing initial pressure build-up by probing the MOSFET pulldown side, and troubleshooting a failed Fuel Pump Output 2.

## Details

**Fuel pump solenoid integration plan:**
ggurov raised the question of how to integrate a fuel pump solenoid profile into the ECU once the solenoid's behavior has been characterized:

- The proposed approach: build the solenoid control profile into the ECU firmware and drive it from a low-cost FET (described as "Amazon-grade FET") on one of the ECU's output channels.
- This implies using a standard low-side switched FET output from the ECU to control the fuel pump solenoid directly, once the duty cycle / frequency profile is known.

**Probing MOSFET for pressure build-up measurement:**
Tera shared an oscilloscope capture of a MOSFET in the fuel pump circuit. Joesphan explained what the probe was measuring:

- The probe was on the **pulldown side** of the MOSFET (the drain or source side that pulls toward ground when the FET switches on).
- The waveform on that node represents the **initial pressure build-up** in the fuel system — as the pump energizes, voltage on the pulldown side reflects the load current transient associated with the pump pressurizing the fuel rail.
- This is a valid technique for observing pump behavior without an inline pressure transducer: the FET switching waveform shape correlates with the pump's load.

**Troubleshooting failed Fuel Pump Output 2:**
Tera reported that Fuel Pump Output 2 had stopped functioning entirely. Joesphan's recommended first step:

- **Use a multimeter to check the MOSFETs.** Start at the hardware level before investigating firmware or configuration.
- Check for:
  - Short between drain and source (failed FET — shorted).
  - Open circuit (blown FET or broken trace).
  - Gate voltage present when output is commanded on (verify ECU is actually driving the gate).
- If the FET tests bad, replace it. If the FET tests good, move to checking the gate drive signal and the ECU output configuration.
