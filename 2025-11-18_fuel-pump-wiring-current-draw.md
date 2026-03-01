# Fuel Pump Wiring: Current Draw, Gauge Requirements, and PWM Control

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @Chemex-joshseek, @MykkH, @ggurov, @Joesphan*

## Summary

Discussion covering real-world fuel pump wiring failures caused by excessive current draw from aftermarket fuel pumps, minimum wire gauge and relay requirements, and the concept of using PWM to control fuel pump speed linked to throttle position or multiple ECU output channels.

## Details

### Factory Wiring Is Insufficient for Performance Fuel Pumps

@Chemex-joshseek noted that factory fuel pump wiring generally cannot handle the current demands of high-flow aftermarket fuel pumps. The wiring in question was described as "dodgy" and inadequate for sustained use.

**Recommended wiring standard:**
- Minimum **14 AWG wire** for the fuel pump circuit
- A **dedicated relay** for the fuel pump (do not share with other loads)

### High-Current Pump Failures in the Field

@MykkH reported a real-world failure with a **Deatschwerks (DW)** high-flow fuel pump:
- The pump drew so much current that it **melted 12 AWG wiring**
- It also **burned out the terminals** inside the fuel pump relay
- The fix was to revert to a **Bosch fuel pump**, which draws significantly less current

@ggurov had a similar experience: excessive current draw from the fuel pump burned the contacts on the **pump connector itself** rather than the relay.

### Key Takeaway on Wire Gauge

These failures highlight that even 12 AWG can be marginal for high-flow pumps. 14 AWG (as mentioned by @Chemex-joshseek) is a minimum for a dedicated circuit with a proper relay, but the wire gauge should be selected to match the actual current draw of the specific pump used. High-flow pumps (e.g., DW series) may require 10 AWG or heavier wiring.

### PWM Fuel Pump Control

Two approaches to PWM control of fuel pumps were discussed:

1. **Multi-channel PWM (conceptual):** @Chemex-joshseek suggested the possibility of using 6 PWM output channels at 50% duty cycle to drive a fuel pump. This approach needs careful consideration of current draw per channel and potential overheating.

2. **Throttle-linked PWM (Joesphan's project):** @Joesphan mentioned configuring a PWM-controlled fuel pump linked to the **accelerator/fuel pedal position** for dynamic fuel pressure adjustment. No specific settings were detailed at this stage of the discussion.
