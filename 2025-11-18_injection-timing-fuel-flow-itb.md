# Injection Timing Effects on Fuel Flow and ITB Systems

*Source: rusEFI Discord, 2025-11-18 | Channel: 1392172039430738111*
*Contributors: @Jordan, @Joesphan, @Robb235, @ggurov*

## Summary

Dyno testing revealed that sweeping injection timing across a wide range (360–80 BTDC) produced approximately a 3% change in measured fuel flow at constant fuel pressure, MAP, and pulse width. The discussion covered how injection timing affects effective fuel delivery, why it matters particularly for ITB (individual throttle body) setups, how to interpret the degree reference convention used in rusEFI, and practical guidance on using consistent delta fuel pressure and EGT as diagnostic tools.

## Details

### Observed Fuel Flow Change with Injection Timing

@Jordan reported from real dyno data:

> "I reviewed some data from an engine on the Dyno. Swinging injection timing from 360-80BTDC and saw a 3% change in fuel flow for the engine, all else being equal. No fuel pressure change, no MAP change, and no PW change."

The observation was confirmed at specific conditions:

> "550* 2700rpm" — @Jordan (in response to whether the 3% change was observed at 550° injection timing)

@Robb235 confirmed the question: "The 3% change in fuel flow was observed at 550* injection timing?"

This is a meaningful finding: changing injection timing alone — while holding all other fuel-related parameters constant — affects actual fuel flow by a measurable amount. The likely mechanism is the relative timing of the injection event to the intake valve opening, which influences charge density and effective fuel preparation.

### Hardware Note: Injector Orientation

During this discussion, @Joesphan noted a separate contributing factor that was found on the engine being analyzed:

> "the injector was mounted upside down"

An upside-down injector will spray with the pattern rotated 180°, potentially directing fuel against the port wall rather than toward the intake valve, which can alter effective fuel delivery.

### Injection Timing Degree Convention in rusEFI

@Joesphan clarified the reference frame used for injection timing angles:

> "so if 360 degrees is tdc at the top of compression stroke, the spray happens during exhaust stroke mostly"

This means in rusEFI's convention, 360° = TDC of the compression stroke (firing TDC). A setting of 360° would inject primarily during the exhaust stroke, which is generally not ideal. The practical injection window for port injection is during the intake stroke, which corresponds to angles between approximately 360° and 720° (or equivalently, negative values relative to intake TDC).

@ggurov noted the general reference:

> "injection timing offset from TDC compression stroke"

### Injection Timing Especially Matters on ITB Systems

@ggurov emphasized that the effect of injection timing is more pronounced on ITB setups:

> "injection timing will make a difference with ITB"

On individual throttle body (ITB) systems, each cylinder has its own short intake tract with minimal plenum volume. The timing of injection relative to the brief intake valve opening window is critical because there is very little time for fuel mixing and no shared intake volume to smooth out timing errors. On a conventional plenum intake, the longer dwell time makes injection timing less sensitive.

### Practical Guidance: Target Consistent Delta Fuel Pressure

@Joesphan provided practical advice for dialing in injection timing in a real-world context:

> "but a good reason to dial injection timing is for consistent fuel pressures like jordan said"

> "from a practical standpoint, target for consistent delta fuel pressure"

Fuel pressure consistency (maintaining a steady differential pressure across the injector across the engine's operating range) is a reliable indicator that the injection system is properly configured. Erratic delta fuel pressure can indicate injection timing events coinciding with pressure pulses in the fuel rail.

### Using EGT as a Diagnostic Tool

@Jordan noted that Exhaust Gas Temperature (EGT) can serve as a diagnostic tool for injection-related issues, with an important caveat:

> "EGT could indicate IF you had repeatable conditions"

EGT is only a reliable diagnostic indicator when all test conditions (RPM, load, ambient temperature, coolant temperature) are held constant between runs. Under repeatable conditions, a change in EGT between injection timing settings can confirm whether the timing change is actually affecting combustion quality.

### Dyno Tuning Note

@ggurov summarized a practical approach to tuning injection timing:

> "tune it on the dyno, these are samples"

The stock rusEFI fuel pressure table is set to a flat value:

> "stock rus table is a flat 410" — @Joesphan

This flat 410 (kPa or mbar depending on context) is a starting point; actual tuning requires adjusting based on the specific injectors, fuel rail, and engine configuration.
