# BMW N54 GDI Injector Calibration Data and Target Lambda Strategy

*Source: rusEFI Discord, 2025-11-25 | Channel: epicEFI dev (1356732771325968630)*

*Contributors: fast335xi (fast335xi), Joesphan (joesphan), ggurov (ggurov), Tera (teraflopping)*

## Summary

BMW N54 GDI injectors have individual calibration codes printed on the injector body. The factory DME uses these codes to look up per-injector trim data. New injectors are considered "self-learning" because production tolerances keep them within 1% of each other, allowing the DME to treat them as matched without stored codes. The OEM fueling strategy uses **target lambda 1.0 at all normal operating points**, switching to **lambda 0.84 at full load** (when the load target / driver demand threshold is crossed). This lambda-based approach works well for GDI because injector delivery is highly predictable with no port/wall-wetting effects.

## Details

### N54 Injector Calibration Codes

- Each N54 injector has a **7-digit IMA (Injector Matching Adaptation) code** printed on its body.
- The DME stores these codes and uses them to look up individual injector flow offsets.
- The relevant DME adaptation tables are (from Tera's engine data):
  - `qdyninjad_ram_xzyl` — dynamic injection quantity per cylinder (primary adaptation map)
  - `Ti_ofs_injad_xzyl` and `Ti_ofs_injad_xzyl_ymod` — injection time offset adaptation per cylinder
- New OEM-supplied injectors are **within 1% of each other** in flow rate, so the DME can auto-learn them without pre-loaded IMA codes.
- The injector data format is **universal to part number** (not serialised to a specific engine VIN), unlike some later BMW systems. Joesphan noted that BMW did move to serial-coded DI injectors on later platforms.
- OEM first-pick binning: BMW receives tightly binned injectors from Bosch; aftermarket buyers get wider-tolerance units.

### Injector Type Note

- N54 uses **solenoid-type GDI injectors** (not piezo).
- Newer GDI platforms (post-N54) moved to piezo injectors which are not auto-learning in the same way.

### Factory Fueling / Target Lambda Strategy

| Operating Condition | Target Lambda |
|---------------------|---------------|
| All normal operation (idle, cruise, part-throttle) | **1.0** |
| Full load (load target reached / driver demand above threshold) | **0.84** |

- The base fuel map is lambda 1.0 everywhere.
- When the driver demand / load target threshold is crossed, the DME richens to **λ 0.84** automatically.
- There are additional **spool fuel target maps** that smooth the transition between the cruise and full-load lambda targets (particularly relevant with VaNOS / cam timing during spool).
- A manual AFR map mode exists as a configurable option but is not the factory default.

### Why Target Lambda Works Well for GDI

- GDI injects directly into the cylinder; there is **no port wall-wetting** effect.
- Injection quantity is highly predictable from injection pressure and pulse width alone.
- This allows the ECU to use an open-loop target lambda approach with high confidence, without the fueling corrections needed for port injection (wall-film, puddle dynamics, etc.).
- ggurov's summary: "target lambda because they can precisely predict how much fuel will get injected — no funky wall-fuel shit, all in."

### Applicability to epicEFI / rusEFI GDI Implementations

- Joesphan asked whether the N54 injector self-learning behaviour applies to Tera's setup (epicEFI GDI8 on a BMW engine) — confirmed yes.
- The lambda strategy described above is the reference for how the OEM handles fueling; the epicEFI GDI implementation is being developed to match this behaviour.
- Port-injection engines use a self-learn algorithm that works differently (adaptive fuel trims based on STFT/LTFT); the injector IMA code lookup is specific to OEM DI applications.
