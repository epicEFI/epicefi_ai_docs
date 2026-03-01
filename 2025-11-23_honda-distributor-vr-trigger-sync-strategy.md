# Honda Distributor VR Trigger Strategy: Single-Tooth Cam Sync, Startup Delay, and VR Signal Combination

*Source: rusEFI Discord, 2025-11-23 | Channel: 1401895238481744052*
*Contributors: Walter R. (@walterronny), Joesphan (@joesphan), ggurov (@ggurov), DCwerx (@dcwerx), SanGawku (@sangawku)*

## Summary

Extended discussion covering the synchronization challenges of using the Honda OBD1 distributor (24/4/1 VR sensor arrangement) with rusEFI/epicEFI on a Proteus ECU. Topics include: the definition and behavior of half-sync vs. full-sync vs. no-sync, why a single-tooth cam trigger causes startup delay, the feasibility of electrically combining two VR signals to create a 4+1 cam pattern, and practical workarounds used in the community (24-1 modification, using only the 4-tooth ring as crank, simultaneous injection mode).

## Details

### Honda Distributor Sensor Architecture

The Honda OBD1 distributor contains three VR sensors on the same shaft:
- **24-tooth ring** (TDC — crank reference, fires every 15 degrees of distributor rotation = 30 degrees crank)
- **4-tooth ring** (CYP — cylinder position, one tooth per cylinder)
- **1-tooth ring** (TDC/cam sync — single pulse per distributor revolution = once per 720 degrees crank)

Because the distributor runs at cam speed (half crank speed), the effective crank-equivalent tooth counts are 24→48 effective, 4→8 effective, 1→sync once per 720 degrees. The 4-tooth ring behaves like a **4+1** pattern when the single tooth is not aligned with the 4-tooth teeth (confirmed by SanGawku and Walter). This makes the distributor equivalent to a Honda K-series style 4+1 cam trigger from the ECU's perspective.

### Half-Sync vs. Full-Sync vs. No-Sync

As defined by Joesphan in this discussion:

- **Half-sync**: ECU knows crank position within 0–360 degrees. It can determine which half of the engine cycle it is in but not which specific cylinder is on compression vs. exhaust stroke.
- **Full-sync**: ECU knows crank position within 0–720 degrees (full two-revolution cycle). Sequential injection and sequential ignition are possible.
- **No-sync** (or running in "no sync" mode): The ECU has not yet established cam/crank correlation. **The engine will not fire in no-sync mode** with a distributor 4+1 configuration. Full sync or no-fire is the behavior.

With a distributor and a 4+1 cam trigger, the ECU requires **full sync** before firing. Simultaneous and batch-fire modes also **only fire after at least half-sync** is established — they do not fire pre-sync.

### Single-Tooth Cam Sync Startup Delay

Walter R. observed a delay between the ECU receiving the first cam rise signal (visible in the rusEFI log) and the actual first ignition event. Investigations by Walter and ggurov identified:

- The delay is in the **trigger decoder code**, not in sensor hardware. The VR conditioner and sensor signal were confirmed clean (Walter's VR conditioner had been validated across 30+ vehicles).
- The cam1 rise count was not visible in TunerStudio's live dashboard even though the signal was active in logs — a known display quirk.
- Walter spent many hours debugging a **rare code path specific to the 1-tooth trigger** configuration that produced unexpected behavior.
- Worst case with a single cam tooth: the ECU may need **two full crank revolutions** to establish sync, because the single tooth only fires once per 720 degrees and the ECU must observe it to determine phase.

### Fast-Start Workaround: 4-Tooth Crank, No Cam

ggurov confirmed that configuring the **4-tooth TDC ring as the crank trigger with zero missing teeth** (pure 4-tooth wheel, no sync pattern) causes the distributor to fire spark and fuel **immediately on the first tooth seen**, before any sync is established. This works because:

- With a distributor providing mechanical sequential ignition, every tooth fires the correct cylinder.
- Simultaneous injection fires all injectors together on the first tooth.
- No sync check is required for the ECU to start contributing spark; the distributor handles routing.
- ggurov: *"distributor starts to fire after the first tooth"* and *"simultaneous injection, 4 tooth 0 missing distributor"*

**Caveat (ggurov):** Batch injection mode may miss the first few revolutions compared to simultaneous mode in this configuration.

### VR Signal Combination Feasibility

Walter R. proposed **electrically joining the TDC (4-tooth) VR output to the CYP (1-tooth) VR output** on the same wire into one ECU cam input, to create a 4+1 cam pattern without modifying the distributor. Community assessment:

- **Walter's friend and AI consultation**: Directly summing two VR (variable-reluctance) analog signals is not straightforward. VR sensors generate AC sine waves; summing them may cause interference, polarity cancellation, or signal amplitude reduction rather than clean additive tooth events.
- **Joesphan**: *"It might work and the signal will be half as strong"* — acknowledges possible amplitude reduction but not ruled out.
- **ggurov**: *"maybe they'll cancel each other out and it will get picked up as 4-1?"* — noted the possibility of destructive interference producing an unintended pattern.
- **Walter's proposed alternative**: Convert one VR signal to a digital square wave (via VR conditioner / MAX9926 or similar), then OR or combine it with the other signal's digital output before feeding to the ECU. This is more reliable than analog summing.
- **DCwerx**: Confirmed a similar approach worked on a Toyota 2JZGE distributor (also 24+1 pattern) using a **MAX9926** VR conditioner treating the combined signal as a **12-1** pattern. The PNPduino uses an LM393-based conditioner for Honda distributors.
- **Conclusion**: Direct analog VR-to-VR combining is not recommended. Digital signal combination after individual conditioning is the correct approach.

### Distributor Trigger Wheel Modification Options

For users willing to modify the distributor to improve sync speed:

| Approach | Description | Notes |
|---|---|---|
| **24-1** | Grind one tooth off the 24-tooth ring | Fast sync, sequential capable via cam signal; common PNP mod |
| **4-tooth only** | Use only 4-tooth ring as crank, no cam | Instant fire on distributor, batch/simultaneous fuel only |
| **4+1 (stock)** | Use 4-tooth ring as cam trigger with 1-tooth sync | Full sync needed before fire; delay on startup |
| **Digital 4+1** | Combine conditioned 4-tooth + conditioned 1-tooth signals digitally | Walter's proposed next test |

DCwerx: *"In the past I always run them batch with 4 teeth only or 24-1 by grinding off a tooth and that can do sequential."*

### Trigger Offset and 90-Degree vs. 720-Degree Trigger Modes

Walter R. attempted to use an **RPM input mode** that was configured for 90-degree trigger intervals instead of the standard 720-degree cycle. This was an attempt to fire before the half-sync event. Joesphan clarified that the standard simultaneous and batch modes still require half-sync before firing, so changing the trigger interval alone does not bypass the sync requirement.

### Practical Recommendation

For Honda OBD1 distributor installations wanting the fastest possible cold start with rusEFI/epicEFI:

1. Configure trigger as **4-tooth crank, 0 missing teeth, no cam input**.
2. Use **simultaneous injection** mode (fires all injectors on every tooth).
3. ECU fires spark on the first tooth detected; distributor routes spark mechanically.
4. Once running, consider adding cam sensor for sequential operation if needed.
5. If cam sync is required for sequential from startup, test digital combination of 4-tooth and 1-tooth outputs via two separate VR conditioners before the ECU cam input.

Reference: rusEFI supported triggers wiki — [All Supported Triggers: Jeep/4-1](https://github.com/rusefi/rusefi/wiki/All-Supported-Triggers#jeep) (4-1 pattern documented; note this is a Toyota/Jeep pattern, not the Honda 4+1).
