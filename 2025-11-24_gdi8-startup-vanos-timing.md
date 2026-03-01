# GDI8 First-Start Tuning: VANOS Off, Lobe Offsets, and Pulse Width

*Source: rusEFI Discord, 2025-11-24 | Channel: tuning-and-software (1356732771325968630)*
*Contributors: Tera (@teraflopping), ggurov (@ggurov), ZeeKay (@its_zeekay)*

## Summary

Early experience getting a GDI8-equipped BMW engine (N-series with VANOS) running on epicEFI for the first time. The key practical steps were: disabling VANOS (VVT) to eliminate valve overlap during initial cranking/startup, targeting a long injector pulse width (~18 ms) to overcome GDI cold-start fuel delivery, and separately using lobe offsets for high-pressure fuel pump (HPFP) pressure building. Loss of the injector timing table is recoverable via File > Restore Point.

## Details

### Getting the GDI Engine to Start

The core challenge with GDI on epicEFI is that the firmware's normal controls are designed for port injection. The mapping of low-side trigger signals to GDI injector driver hardware introduces timing complexity that is not yet fully documented.

**Disable VANOS/VVT for initial startup:**
The first confirmed step that enabled initial combustion was disabling VANOS (VVT). With VANOS active, valve overlap was preventing the engine from starting. Turning off VVT eliminated the overlap condition and allowed the engine to fire.

- In epicEFI, disable the VVT/cam phaser control during initial GDI bring-up.
- Once the engine starts and runs, VVT can be re-enabled and tuned.

**Target pulse width — approximately 18 ms:**
GDI injectors operate at much higher fuel pressure than port injectors but the effective flow window per cycle is constrained by cam geometry. A pulse width of approximately **18 ms** was the working target for initial fueling on this build.

- The normal port-injection pulse width scaling does not translate directly to GDI; expect to start with much longer requested pulse widths than you would use for port injection.

**HPFP pressure building — lobe offsets:**
Building adequate rail pressure from the HPFP requires configuring lobe offsets (the cam-based pump drive timing). Lobe offset configuration is separate from injector pulse width tuning. The workflow is:

1. Get the engine running first (low pressure, long pulses).
2. Then configure HPFP lobe offsets to build adequate high-pressure fuel rail pressure.
3. Iterate from there.

### Recovering a Lost Injector Timing Table

Tera encountered a situation where the injector timing table disappeared from the tune — likely caused by a crash or a tune reload that did not include that table.

**Recovery procedure:**
1. In TunerStudio: **File > Restore Point**
2. This loads a recent automatic snapshot of the tune, which may contain the missing table.

**Prevention:**
- Copy-paste the current tune file to a dated backup before making significant changes. As Joesphan noted: *"copy and paste currenttune is most reliable method, it's usually to save myself from myself."*
- Crashes during startup attempts (e.g., while trying to start a GDI engine) can wipe table data; keep frequent manual backups.

### Notes on the GDI8 / epicEFI Bring-Up Process

- There is currently no comprehensive documentation for GDI bring-up on epicEFI. Tera's work is exploratory.
- The interaction between low-side trigger signals from the ECU and the GDI8 hardware driver is not yet fully characterized.
- A separate feature request for dual-bank HPFP control and dual rail pressure sensing is in progress (see: `2025-11-24_dual-bank-hpfp-fuel-pressure.md`).
