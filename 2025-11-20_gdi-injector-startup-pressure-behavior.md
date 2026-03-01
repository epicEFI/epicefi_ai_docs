# GDI High-Pressure Injector Startup Pressure Behavior

*Source: rusEFI Discord, 2025-11-20 | Channel: 1356732771325968630*
*Contributors: fast335xi (@fast335xi), Joesphan (@joesphan)*

## Summary

Observation of a GDI (gasoline direct injection) high-pressure fuel system targeting elevated rail pressure for approximately 45 seconds on startup before settling to normal operating pressure, and discussion of diagnosing this via injector serial number data.

## Details

**Observed behavior:**
- On cold start, the high-pressure fuel system targets **2000 psi** for approximately **45 seconds**.
- After the warm-up period, pressure settles to the normal operating target of **750 psi**.
- This elevated startup pressure is consistent with OEM GDI control strategies that use higher rail pressure during cold start to improve atomization and reduce wall wetting before the engine and fuel system reach operating temperature.

**Diagnostic approach — injector serial number data:**
- Joesphan noted that injector-specific calibration data may be encoded in or linked to the injector serial number.
- If the injector serial number can be decoded, it may reveal the exact pressure-flow characterization, dead time tables, and any OEM pressure schedule programmed into the original ECU strategy.
- This data would allow rusEFI/epicEFI to replicate the OEM fueling strategy accurately for GDI applications.

**Context:**
This discussion is relevant to GDI or dual-injection engine conversions using epicEFI, where the high-pressure pump control and fuel pressure target tables must be configured to match the physical behavior of the OEM or aftermarket GDI injectors. The 2000 psi → 750 psi transition suggests a warm-up fuel pressure map is needed in the configuration.
