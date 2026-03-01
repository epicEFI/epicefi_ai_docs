# Injection Timing Angle Reference and Starting Points

*Source: rusEFI Discord, 2025-11-21 | Channel: epicEFI dev/support (GDI build thread)*
*Contributors: ggurov, Tera, Joesphan, SanGawku, matt (FOME)*

## Summary

When configuring injection timing (injection angle) in rusEFI/epicEFI/FOME, the reference point is the ignition falling edge at 0 degrees. Setting an injection advance value of -180 places the start of injection at Bottom Dead Center (BDC). Honda factory tunes typically use values around -350 to -400 degrees for direct injection. During initial bringup, -180 is a recommended first starting point.

## Details

**Reference frame:**

- Ignition falling edge = 0 degrees (the reference point for injection timing).
- The injection advance table is a 720-degree window for a 4-stroke engine.
- Negative values move the injection event earlier relative to ignition.

**Key angle positions:**

| Injection Advance Value | Approximate Injection Timing Position |
|------------------------|--------------------------------------|
| 0                      | At ignition falling edge (TDC) |
| -90                    | Bottom Dead Center (BDC) compression stroke |
| -180                   | Bottom Dead Center — recommended starting point |
| -350 to -400           | Honda factory GDI calibration range |

**Starting point guidance:**

- FOME developer guidance (via matt): set the initial injection advance to **-180** as a "middle of injection" starting point.
- SanGawku confirmed Honda uses approximately **-350 to -400** degrees on most of their direct-injected cars.
- At higher RPM, injection windows compress; values that work at idle may not be appropriate across the full RPM range.

**Diagnosis approach:**

- If plugs are sooty/dry (not wet), the charge may be firing before adequate fuel is injected — suspect injection timing too late or fuel delivery problem rather than too-rich condition.
- If plugs are wet, injection is occurring but ignition is not lighting the mixture.
- Setting a very early injection angle (e.g., -400) restored fuel smell at the intake on a GDI test case, indicating fuel was actually reaching the cylinder.

**TunerStudio display:**

- In the injection advance visualization, moving the value more negative shifts the gray injection band to the left (earlier in the cycle).
- A value of -180 visually moves the injection well before the spark event, which is the correct behavior for most DI applications.

**Related settings to verify:**

- `top ign cyl1` — ignition output assigned to top (TDC) for cylinder 1.
- `bottom inj cyl1` — injection output assigned to bottom (BDC) for cylinder 1.
- All cells in the injection advance table should be verified — setting all to 0 means injection fires at ignition falling edge, which is generally incorrect for DI.
