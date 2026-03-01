# Programmable Output Blinking Using Time-Base Variables in epicEFI

*Source: rusEFI Discord, 2025-11-25 | Channel: 1401895238481744052*
*Contributors: mynameisdeleted (@mynameisdeleted), ggurov (@ggurov), Joesphan (@joesphan)*

## Summary

epicEFI's programmable output system exposes a `seconds` time-base variable that can be used with bitwise operations to toggle outputs at a fixed rate without Lua scripting. A 1 Hz blink (useful for turn signals, hazard lights, or status indicators) is achieved with `seconds & 1`. Sub-second rates require a fractional time base that was identified as a future addition.

## Details

**The problem:** A user wanted an output pin that blinks a few times per second to drive the ground side of turn signal or hazard lights.

**Approach 1 — Lua / Lia scripting (Time loop):**
Running the blink logic in a Lua/Lia Time loop with a delay is the scripted approach. Joesphan confirmed: "You run it on a Time loop, then run a delay or something." This works but requires a script.

**Approach 2 — Programmable output expression (no script):**
ggurov noted that the programmable output configurator already provides a `seconds` variable that increments every second. Using the bitwise AND operator:

```
seconds & 1
```

This expression evaluates to `1` on odd seconds and `0` on even seconds, toggling the output once per second (0.5 Hz effective toggle, 1 Hz full blink cycle). To additionally gate the output on a button or input condition:

```
seconds & 1 & (MEGA_EPIC_D22_D37 & <bit>)
```

where `MEGA_EPIC_D22_D37 & <bit>` is the bitmask for the enable input (e.g., a turn-signal stalk switch). Joesphan's reaction: "Oh, we have a time base now. That's insane."

**Sub-second rates:**
For faster blinking (typical turn signals blink at ~1.5 Hz or ~67% duty), ggurov acknowledged that fractional second time bases (0.5 s, 0.25 s) were not yet available but should be added:

> "I'll have to add fractional second time base" — ggurov
> "Half second, quarter second" — planned additions

**Alternative — General PWM (GPPWM):**
For applications requiring precise duty cycle control or sub-second rates, ggurov pointed out that General Purpose PWM tables (GPPWM) are another option. A GPPWM output can drive a coil and injector simultaneously and supports millisecond-level timing. However, GPPWM is better suited for continuous PWM (fans, solenoids) than a simple blink gate tied to a switch.

**Practical note:** The `seconds & 1` programmable output technique requires no Lua license or scripting overhead and is available in the standard programmable output configuration UI.
