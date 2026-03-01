# Hall vs VR Cam/Crank Sensor Identification and Wiring

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Jmoneus (@jmoneus), Joshan Lu (@joshseek_30236_07984)*

## Summary

Practical guide to identifying whether a cam or crank position sensor is Hall-effect or Variable Reluctance (VR) type, including multimeter test methods, wiring differences, power supply requirements, and how sensor type affects ECU input configuration. Audi/VW 3-wire VR sensor variant also covered.

## Details

**Why it matters:**
Hall and VR sensors connect differently to the ECU. Hall sensors are active devices requiring an external power supply and are wired to a digital input with a pull-up. VR sensors are passive coils wired to a differential VR input. Misidentifying the type leads to incorrect wiring and no-start conditions.

**Identifying sensor type with a multimeter:**

*VR sensor (passive coil):*
- Measure resistance across the two signal pins — a VR sensor is a single winding and will show equal resistance in both directions (e.g., ~15 Ω measured one way, ~15 Ω measured the other way).
- The third wire on a 3-wire VR sensor (found on some Audi/VW applications) is a shield, not a power or signal wire. There should be no measurable resistance between the shield wire and either signal pin.
- A 2-pin sensor is unambiguously a VR sensor.

> "VR is a single winding around a core. Resistance will be the same two ways across 2 pins — so 15 ohm one way, and 15 ohm the other way. And nothing between those pair pins and the 3rd." — ggurov
> "If it's 2-pin, it's 100% VR." — ggurov

*Hall sensor (active):*
- Power it up from its factory circuit and measure the supply and ground pins — you will see a definitive 12V supply and a ground reference on two of the three pins.
- The third pin is the signal output (open-collector, switches between supply and floating/low).
- Alternatively, probe the stock vehicle wiring harness while the engine is cranking to observe actual signal transitions.

> "Hall stock will have a 12V supply and a ground — so 2 pins giving definitive readings, with the 3rd pin being the signal." — ggurov

**Probing stock electronics as an alternative:**
Rather than bench-testing sensors in isolation, probing the stock ECU connector or harness while cranking the engine is a reliable way to identify signal type and polarity directly.

**Hyundai-specific example (2007 Elantra 2.0 / 2008 Accent 1.6):**
- Hyundai Accent 1.6 crank sensor: VR type
- Hyundai Elantra 2.0 crank sensor: Hall-effect type
- If both Hall sensors are available on the engine, prefer Hall — simpler wiring, no VR differential input concerns.

**Hall sensor wiring:**
- Supply: 12V from an ignition-switched relay (same circuit as ECU). Hall sensors draw very little current; a shared relay with the ECU is fine.
- Ground: chassis or sensor ground as per factory diagram.
- Signal: ECU cam/crank input (digital/Hall input, internally pulled up).
- Do NOT supply Hall sensors with 5V unless the factory diagram specifically calls for it. Factory hall sensors on cam/crank are almost universally 12V-supplied.

> "Those don't need much power. You power those 12V from the ign key, or whatever relay controls it." — ggurov
> "5V is for TPS/MAP." — ggurov

**VR sensor wiring:**
- Two signal wires connect to the ECU's dedicated VR differential input (VR+, VR−). No external power supply needed.
- On 3-wire Audi/VW VR sensors: connect the shield wire to chassis ground at the ECU end only to avoid ground loops.

**ECU input/output terminology:**
The cam and crank sensor signal pins are listed as "outputs" in the ECU's pinout — this means the ECU outputs nothing on those pins; the sensor drives them. From the ECU's perspective, they are inputs receiving data. The terminology can be confusing because the pin is labeled relative to signal flow direction into the ECU.

> "Output from the ECU point of view means it's giving a signal out to something, while inputs from the ECU POV is something that sends data into it, such as temp." — Joshan Lu

**Hall sensor preference:**
Hall-effect sensors are preferred over VR when both options are available: they provide a clean digital square wave without the zero-crossing processing and threshold tuning that VR inputs require.

> "Hall — that's easier and less bullshit." — ggurov
