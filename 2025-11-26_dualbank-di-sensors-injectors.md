# Dual-Bank Direct Injection: Sensor Configuration and Injector Selection

*Source: rusEFI Discord, 2025-11-26 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Joesphan (@joesphan), fast335xi (@fast335xi)*

## Summary

Discussion around enabling tested dual-bank DI support in epicEFI/rusEFI, focusing on which sensors are bank-specific vs. shared, and a comparison of BMW vs. Toyota high-pressure direct injectors. The BMW N63TU (twin-turbo V8) and S58 (I6) were the primary reference engines.

## Details

**Dual-bank DI status:**
As of 2025-11-26, dual-bank direct injection was being actively tested in epicEFI. The project reached a milestone described as "TESTED DUALBANK DI," indicating the feature moved from experimental to a state with confirmed test coverage.

**Bank-specific sensors (BMW V8 example — N63TU / S58):**
The only sensors that are truly bank-specific on a dual-bank BMW engine are the oxygen sensors:
- 4 O2 sensors total (2 per bank, pre- and post-cat)
- Shared across both banks: single fuel rail pressure sensor, shared high-pressure fuel pump (HPFP)
- Exception: the BMW S58 uses 2 HPFPs on a common rail for higher fuel delivery

**Coolant temperature considerations for dual-bank:**
On a long inline-6, a single CLT sensor may not represent both ends of the block accurately. Per-cylinder or per-end CLT sensing was raised as an option for engines where front-to-rear thermal gradients are a concern, though not standard practice.

**N54 / S55 / S58 common practice:**
These BMW inline-6 engines technically have a "second bank" of DI injectors, but in aftermarket tuning they are almost universally consolidated to a single bank in software. Only V8 (and similar) configurations genuinely require dual-bank DI logic.

**Injector comparison — Toyota vs. BMW GDI injectors:**
For BMW high-pressure direct injection applications, Toyota-sourced injectors are preferred over OEM BMW injectors:
- Toyota injectors rated to **350 bar** operating pressure
- BMW OEM injectors rated to **250 bar** operating pressure
- Toyota injectors are considered mechanically superior for modified/high-output applications in BMW GDI engines

**Relevance to rusEFI configuration:**
When configuring dual-bank DI in rusEFI/epicEFI:
- O2 sensor inputs must be assigned per-bank
- Fuel rail pressure and HPFP control can typically be treated as single shared inputs/outputs
- Injector characterization tables should reflect the actual injector fitted (Toyota vs. BMW spec)
