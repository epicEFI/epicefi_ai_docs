# NB Miata epicEFI Install: Trigger Signal Troubleshooting Session

*Source: rusEFI Discord, 2026-02-28 | Channel: 1356732771325968630*
*Contributors: @Bloopdog, @MartyMotorsports, @Joesphan, @ggurov*

## Summary

A user (@Bloopdog) installing epicEFI on a 2002 NB2 Mazda Miata worked through a live troubleshooting session with an experienced NB Miata epicEFI user (@MartyMotorsports) and two developers (@Joesphan, @ggurov). The core problem was noisy trigger signals — particularly the cam sensor — resulting in no RPM reading and therefore no spark output. The session produced a concrete diagnostic procedure and several NB-specific wiring tips.

## Details

### Hardware and Firmware

- Board: STM32-H7 adapter board in Arduino Mega 2560 footprint, sourced from DCwerx (ggurov design, USB-B connector with SD card)
- Firmware: standard `144h7` build. A custom "black fw" build was tried first but did not work well for this application.
- MartyMotorsports was running the same adapter board platform but with an older "black" board (his own red board also confirmed working).

### Pin Assignments (DCwerx H7 adapter, NB Miata)

ggurov confirmed the correct pin assignments for the NB Miata on the DCwerx H7 adapter:
- Crank sensor: **PD3** (D19 function)
- Cam sensor: **PD4**

These do not map 1:1 with MartyMotorsports' board; users with different board revisions should confirm with ggurov if uncertain.

### Trigger Settings

- Trigger pattern: **Miata NB2** preset (correct for 2001–2005 NB Miata)
- Edge polarity: **Falling** edge confirmed working by MartyMotorsports; rising edge should also work. Bloopdog had tried falling on both crank and cam without resolution.
- NB crank wheel: 4-tooth pattern with approximately 70°/110° spacing (asymmetric, not 4-1 missing tooth)
- NB cam wheel: 180° pair with one extra tooth positioned closer to one of the pair

### Diagnostic Procedure for No-RPM Condition

The recommended approach when there is no RPM signal:

1. Remove all spark plugs — compression pulses can interfere with trigger readings and also masquerade as signals.
2. Disable fuel injection via the "flood clear" mode or by disabling injectors in software (or physically unplug injectors) to avoid flooding while cranking without plugs.
3. Capture a **trigger log** in TunerStudio (or the epicEFI tuning software) while cranking for approximately 15 seconds.
4. Post the trigger log for analysis.

When the trigger log was reviewed, the result was:
- Cam signal: clean — described as a "180° pair and an extra tooth closer to one" (correct NB pattern)
- Crank signal: "wonky" — double-trigger pulses visible, consistent with electrical noise
- Sync was present early in the log but lost partway through

Joesphan noted that the tachometer output is downstream of RPM; do not try to debug tach output until RPM is reading correctly.

### Root Cause: Cam Sensor Noise

After inspecting the trigger log, the cam sensor signal was identified as noisy. MartyMotorsports identified a known NB Mazda issue: **the cam sensor connector tends to back off** (loosen) from the sensor body. This is a common failure mode, especially on cars that have had cam seals replaced (Bloopdog had just completed a cam seal job). First step: unplug and firmly reseat the cam sensor connector.

The NB trigger signals are inherently noisy and the rusEFI/epicEFI firmware includes a specific software filter for this. However, the DCwerx H7 adapter board does not include a hardware VR conditioner — the NB sensors are Hall effect, so no VR conditioner is needed, but the Hall signals still benefit from filtering. MartyMotorsports noted that on his setup he has heavier noise filtering engaged in software ("speedy code").

### Wiring Best Practices for Trigger Noise

MartyMotorsports' working NB Miata setup used these practices to keep trigger signals clean:
- Trigger wires (crank and cam sensor leads) **routed separately from the main harness loom**
- Trigger wires **shielded** (screened cable or shielded twisted pair)
- All non-engine-performance wiring removed from the engine bay

Bloopdog's wiring was in the stock OEM harness location (bundled with everything else), which is more susceptible to noise pickup.

### Capacitor Filter for Noisy Trigger Input

Joesphan suggested adding a small ceramic capacitor across the trigger input signal line as a simple hardware noise filter:
- Place the capacitor **between the signal wire and ground** at the ECU input
- Suggested starting range: **1 nF to 10 µF ceramic capacitors** (a ceramic capacitor assortment kit covers this range)
- Trade-off: larger capacitance gives better noise rejection but rolls off high-frequency response, which matters at high RPM. For typical street use this is unlikely to be a problem.
- Recommended approach: try capacitors from the low end of the range upward; remove and retest once the correct value is found, then incorporate it into the permanent harness or propose it for the firmware/hardware dev pipeline.

### Fuel Injection Mode

With a noisy or uncertain cam signal, running crank-only (batch/simultaneous injection) was discussed as a way to verify the rest of the system. MartyMotorsports noted that his car started poorly on simultaneous injection but worked well on sequential. The NB trigger pattern is symmetric enough that crank sync is possible depending on implementation. The recommendation was to get the cam signal clean rather than work around it.

### Ignition / Spark

Bloopdog could manually test-fire the ignition coils (spark visible when commanded), but no spark occurred during cranking because the ECU had no RPM reading. This is expected behavior — spark is gated on RPM. Power to ignition coils is shared with the ECU main fuse; a separate ignition-specific fuse does not exist on this platform, so checking fuses would not isolate a spark issue.

### Planned Next Steps (end of session)

1. Reseat or replace the cam sensor (Bloopdog had a spare; Mitsubishi Evo 8 cam sensors are confirmed compatible)
2. Check cam sensor wiring — inspect connector locking tab engagement
3. If cam sensor swap does not fix the issue, revisit trigger wiring routing and shielding
4. If still stuck: contact ggurov or DCwerx for board-specific pinout differences; may fall back to the original Mega ECU temporarily to verify the engine is fundamentally healthy

## Notes

- The DCwerx H7 board and ggurov's own board are related designs but have different pin mappings. Users of the DCwerx board should not assume 1:1 compatibility with community-shared pinout diagrams for ggurov's board.
- The NB cam sensor connector backing off is a recurring issue, especially after valve cover or cam seal service. Always check connector seating after this type of work.
- NB2 Miata = 2001–2005 model years. NB1 (1999–2000) trigger configuration may differ slightly.
- Firmware version `144h7` refers to the STM32H7 build. The "black fw" variant is a different build that may not be appropriate for all hardware.
