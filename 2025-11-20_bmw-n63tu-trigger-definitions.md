# BMW N63TU VVT Trigger Definitions

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: Tera (@teraflopping), Joesphan (@joesphan), ggurov (@ggurov)*

## Summary

The BMW N63TU engine requires custom VVT (cam) trigger definitions to be added to epicEFI/rusEFI. The crankshaft uses a standard 60-2 wheel; only the camshaft trigger is non-standard. A community implementation was found in a FOME fork and is being integrated into epicEFI.

## Details

**Engine:** BMW N63TU (twin-turbocharged V8, used in F-series 7-series/X5/X6 and related)

**Crank trigger:** Standard 60-2 tooth wheel — no custom definition required; use the existing 60-2 trigger type.

**Cam (VVT) trigger:** Non-standard pattern requiring a dedicated definition.
- A working implementation was authored by community member Creesic (GitHub: https://github.com/Creesic) on 2025-08-18.
- Reference PR/branch: https://github.com/FOME-Tech/fome-fw/compare/master...Creesic:fome-fw-Creesic:N63trigger
- Implementation file: `trigger_bmw.h` / `trigger_bmw.cpp`, function `initializeVvtN63TU(TriggerWaveform *s)`

**Signal event format:**
The trigger waveform events use `addEvent720()` calls. Signal polarity convention:
- `true` = rising edge
- `false` = falling edge

Example from the implementation:
```cpp
s->addEvent720(246, true, TriggerWheel::T_PRIMARY);
```

**Integration status (as of 2025-11-20):**
- ggurov confirmed the N63TU trigger definition would be added to epicEFI firmware.
- Tera (Creesic) authored the original implementation targeting their BMW 750Li (N63TU, twin-turbo V8 GDI).

**Related notes:**
- The "yolosync" feature (guess-and-check sequential sync, a batch-to-sequential automatic sync mechanism) was also discussed as potentially useful for GDI engines like the N63TU where cam sync may be difficult to establish reliably.
- For cam trigger signal polarity when implementing custom patterns: `true` = rising, `false` = falling (confirmed by Tera from FOME codebase).
