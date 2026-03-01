# Ethanol Content Handling and Stoichiometric Ratio Correction

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov (@ggurov), Tera (@teraflopping)*

## Summary

rusEFI handles ethanol content incorrectly when no flex fuel sensor is present: users typically enter 14.7:1 as the stoichiometric AFR, but pump gasoline in most markets is E10 (10% ethanol), which has a stoich of approximately 14.1:1. epicEFI addresses this with an ethanol content override input and a global fuel multiplier, allowing correct stoich-based fueling even without a dedicated flex fuel sensor.

## Details

**The rusEFI stoich handling problem:**
- When no ethanol content sensor is fitted, rusEFI assumes a user-configured fixed stoich value.
- Most users enter 14.7 (stoich for pure gasoline / E0).
- However, pump fuel in most markets is E10 (10% ethanol blend), which has a stoich of ~14.1:1.
- Running 14.7 as stoich on E10 fuel causes the ECU to run lean relative to the actual charge — the fuel calculation is incorrect from the first principles.
- ggurov: "rus gets it wrong" / "no ethanol sensor, it runs 'stoich'" / "you don't get e0 from the pump anymore, you get e10."

**epicEFI improvements:**
The epicEFI firmware adds:
1. **Current ethanol content override** — an input/configurable field to specify actual fuel ethanol percentage without requiring a physical flex fuel sensor.
2. **Global fuel multiplier ("yolo global fuel multiplier")** — a direct fuel scaling factor, adjustable in real-time from the button box, for rapid overall fueling adjustment.
3. **MAF/MAP hybrid mode** — available in epicEFI.
4. **%BARO Y-axis override** — allows barometric pressure to be used as a VE table axis.

**Accelerator enrichment ("foot down"):**
- ggurov confirmed that epicEFI correctly handles accelerator enrichment: "foot down means proper enrichment."
- This refers to correct Accel Enrichment (AE) behavior — when the throttle is depressed rapidly, the system delivers appropriate transient fuel enrichment.

**VE table global scaling:**
- A common tuning technique is multiplying the entire VE table by a scalar to shift overall fueling up or down uniformly across all load/RPM points.
- This is available in epicEFI and was noted by Tera as a preferred quick-calibration method.

**Practical guidance:**
- If running E10 pump fuel without a flex sensor, set stoich to ~14.1 (not 14.7) for correct base fueling.
- If fuel blend varies, fit a flex fuel sensor and enable ethanol content sensing for automatic stoich correction.
- The global fuel multiplier in epicEFI provides a coarse real-time adjustment independent of the VE table.
