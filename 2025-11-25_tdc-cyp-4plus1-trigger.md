# Combining TDC and CYP Signals for a 4+1 Trigger Pattern

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: Walter R. (@walterronny)*

## Summary

Walter R. proposed combining a 4-stroke TDC (Top Dead Center) signal with a 1-stroke CYP (Cylinder Position) signal to create a composite "4+1" trigger wheel pattern. The combination would need to be processed in digital form and could serve as a viable trigger configuration for engine management.

## Details

**Concept:**
- TDC 4t signal: a signal that fires once per TDC event in a 4-stroke cycle (4 pulses per 2 crankshaft revolutions on a 4-cylinder, for example)
- CYP 1t signal: a single pulse per engine cycle used for cylinder identification
- Combining these two produces a "4+1" pattern — analogous to the common 36-1, 60-2, or multi-tooth-with-missing-tooth trigger wheel designs

**Why this matters:**
- The 4+1 pattern gives the ECU both high-resolution crank position (from the 4t TDC signal) and absolute cylinder reference (from the 1t CYP signal)
- This enables sequential fuel injection and ignition, as the ECU can determine which cylinder is on its compression stroke

**Implementation note:**
- Walter noted the combination "would work when it is converted to digital" — meaning the analog signals from the individual sensors should be digitized (via comparator or ECU digital input conditioning) before being combined or interpreted
- This is consistent with how rusEFI/epicEFI handles trigger inputs: the firmware expects clean digital square-wave signals from the trigger wheel

**Relation to existing trigger patterns:**
- rusEFI/epicEFI already supports many trigger wheel configurations
- A custom 4+1 pattern from combining existing OEM sensors on a Honda-style (TDC + CYP) engine is a recognized approach for running sequential injection on engines that originally used distributor-based ignition
