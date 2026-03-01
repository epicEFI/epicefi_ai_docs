# Hardware Tips: Soldering Technique, HT Lead Misfire Diagnosis, Fan Controller Driver Issue

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @Chemex-joshseek, @Joesphan*

## Summary

Three brief practical hardware notes from the same channel session: a recommended technique for soldering SMD/through-hole components using oven reflow, a quick misfire diagnosis resolved by testing and replacing a faulty high-tension (HT) ignition lead, and a reported overheating problem traced to a fan controller driver issue.

## Details

### Soldering Technique: Flux and Oven Reflow

For assembling rusEFI/epicEFI PCBs or similar boards with surface-mount components:

> "flux the whole board and put the components in the oven"
> — @Chemex-joshseek

Recommended process:
1. Apply flux paste across the entire board (or at least all pads)
2. Place components in position
3. Run the board through a reflow oven (or toaster oven with a reflow profile)

This technique ensures even solder joints and is the standard approach for SMD assembly. Using flux before oven reflow improves solder flow and reduces the risk of dry joints or bridging.

### Engine Misfire: Faulty High-Tension Lead

A simple but overlooked cause of engine misfire is a defective ignition HT (high-tension) lead:

> "i bought the car tested all the HT leads and found one was not good, replaced it and bam fixed running fine"
> — @Chemex-joshseek

Diagnosis approach:
- Test each HT lead individually (resistance test with a multimeter, or swap-test)
- A lead that measures open-circuit or excessively high resistance is faulty
- Replacing the bad lead restored normal engine operation immediately

This is a worthwhile first check before diagnosing ignition timing, coil, or ECU-related causes of misfire.

### Fan Controller Driver Issue Causing Engine Overheating

A user traced an engine overheating problem to the fan controller driver:

> "so apparently it was just a fan controller driver issue"
> — @Joesphan

When an engine is overheating and the cooling fan is not activating as expected, the fan controller output driver (MOSFET or relay driver stage) should be checked. A failed driver can prevent the fan from running even when the ECU is commanding it on, resulting in overheating under conditions where the engine relies on the electric fan.
