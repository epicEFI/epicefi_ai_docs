# Launch Control: RPM Cut Behavior in Static vs. Dynamic Launch Modes

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Tera (@teraflopping)*

## Summary

Clarification of how the RPM cut and RPM window parameters interact in rusEFI/epicEFI launch control, and the difference between static and dynamic launch modes.

## Details

**The RPM window parameter:**

The "RPM window" setting defines the lower threshold of the rev limiter cut — the dead-band below the target launch RPM where no cut is applied. The cut activates as RPM rises toward the target, with the window defining the transition point.

**Static launch mode:**

In static launch mode, the launch RPM target is a fixed, user-configured value.

- At the **launch RPM target:** full cut applied (both fuel and/or ignition cut, depending on settings).
- At **launch RPM − window:** no cut applied.
- Between those two values: hysteresis / partial cut behavior.

ggurov's description: "static launch, rpm is full cut, rpm-window is no cut."

**Dynamic launch mode:**

In dynamic launch mode, the launch RPM target is set at the moment the launch button is pressed, captured from the engine's current RPM at that instant.

- At **launch RPM + adder:** full cut.
- At **launch RPM − window:** no cut.
- The "adder" is a configurable offset above the captured launch RPM.

ggurov's description: "in dynamic, button sets the launch rpm from current rpm."

**Practical implication:**

Dynamic launch allows the driver to set the 2-step RPM on-the-fly by holding the launch button at the desired RPM. This is useful for drag launches where the driver wants to build boost at a specific RPM that may vary by pass or condition. Static launch is simpler and appropriate when a fixed rev limit is always desired.

Tera's question about whether the launch window creates a "smooth 2-step" was answered by ggurov confirming the window defines the lower (no-cut) threshold — so between `launch − window` and `launch` there is an RPM band where the cut transitions, which softens the 2-step behavior compared to a hard on/off switch.
