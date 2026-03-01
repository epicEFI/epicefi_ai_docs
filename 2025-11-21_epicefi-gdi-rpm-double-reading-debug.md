# epicEFI GDI Setup: Double RPM Reading, Sync Loss, and Ignition Requirements

*Source: rusEFI Discord, 2025-11-21 | Channel: epicEFI dev (1356732771325968630)*

*Contributors: teraflopping (Tera), joesphan, ggurov*

## Summary

During live testing of a GDI engine on epicEFI firmware, the system was reporting RPM at double the actual value. The root cause was traced to the RPM source being set to a cam-based trigger while a crankshaft trigger wheel (60-2) was in use. Additionally, the GDI mode in epicEFI requires ignition synchronization to be enabled. The diagnostic workflow involved logic analyzer capture of crank/cam/ignition/injector signals, and comparing behavior between epicEFI and FOME firmware builds.

## Details

### Double RPM Reading Root Cause

Tera observed that the engine was physically running at approximately 2,000 RPM while epicEFI reported ~4,000 RPM in TunerStudio. The cause:

- The RPM source setting was on a cam-based input path
- The camshaft rotates at **half the speed of the crankshaft** (1 cam rotation per 2 crank rotations on a 4-stroke engine)
- If the firmware counts cam teeth as if they were crank teeth, the computed RPM will be **doubled**
- Joesphan confirmed: "if that's 'on camshaft' then it does 2x speed, because camshaft spins 1/2 crank"

The fix guidance from ggurov was to use a 60-2 crank trigger wheel in "yolo" (sync-by-trying) mode with no cam input active, to isolate the variable:

> "yolo without cams and 60-2" — ggurov

This double RPM reading also explained the apparent sync losses and misfires: the ECU's internal timing was offset relative to actual engine position.

### GDI Mode Requires Ignition Sync Enabled

When attempting to disable GDI (to fall back to wasted spark for testing), Tera encountered a persistent error:

> "it wants me to have sync on for ignition if I'm using GDI" — Tera

Even with injection disabled and cam lobes set to 0, the GDI error persisted. ggurov acknowledged this as a known firmware constraint and planned to remove the blocking error check. The setting "require cam sync" must be enabled when operating in GDI mode.

As a workaround for testing with GDI disabled:
1. Define a pin for the cam sensor input (even if not physically used)
2. Set sync mode to "yolo" (sync-by-trying)
3. Enable "require cam sync"

### Logic Analyzer Diagnostic Workflow

To compare timing behavior between epicEFI and FOME firmware, the recommended procedure was:

1. Hook up a logic analyzer (e.g., Saleae) to: **crank sensor**, **cam sensor**, **ignition output** (single cylinder), **injector output** (single cylinder)
2. Capture logs under both firmware builds
3. Compare ignition and injection timing relative to the crank missing tooth

> "we will see the difference in timing with relation to crank and missing tooth, so can see how all of that matches up" — ggurov

Tera's Saleae was not on-site during testing, making this comparison deferred. The recommendation was to order additional low-cost analyzers (~$7.99/unit) for bench availability.

### GDI Backpressure Calculation Concept

During the same session, Joesphan raised a theoretical approach to GDI injector compensation:

- At 100 bar HPFP rail pressure, in-cylinder backpressure at the injector tip varies with piston position
- Backpressure ranges from roughly atmospheric (at bottom dead center) up to maximum compression pressure (at TDC)
- By calculating piston position from crank angle, the ECU could theoretically compute instantaneous backpressure at the injector tip
- This calculated delta pressure could enable running the injector like a **deadhead system**, compensating injection pulse width dynamically for in-cylinder pressure
- Joesphan estimated backpressure variation affects fueling by no more than ~10% at 100 bar rail pressure
- Crank offset (non-linear piston motion) must also be accounted for in the calculation

This was a design discussion rather than an implemented feature.

### Firmware Version Bisection

When double RPM persisted across firmware rollbacks (11-16, 11-15, 11-5 releases), ggurov concluded the issue was not firmware-specific but a configuration problem:

> "nah, it was the n62 3 cam — I bet it's the cam" — ggurov

On a BMW N63 engine (which has 3 cam sensors), the cam input configuration is more complex. ggurov's plan was to review logs and disable the blocking GDI error, since 60-2 crank trigger physically cannot produce a double RPM reading on its own.
