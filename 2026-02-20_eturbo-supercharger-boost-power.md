# E-Turbo / Electric Supercharger Boost Target and Power Requirements

*Source: rusEFI Discord, 2026-02-20 | Channel: 1411925674498719775*
*Contributors: @mynameisdeleted*

## Summary

A community member working on an electric turbo/supercharger integration with rusEFI documented their target airflow, boost pressure goals, and power system requirements. The setup targets 238 CFM at 8-10 psi boost using a 48V electric compressor driven by a brushless motor, requiring approximately 700W of electrical power from a 48V regulator that engages above 4,000 RPM.

## Details

### Airflow and Boost Targets

| Parameter | Value |
|---|---|
| Target volumetric efficiency | 150% (with boost) |
| Target airflow | 238 CFM |
| Target boost pressure | +8 to +10 psi |
| Compressor type | E-turbo (electrically driven) |

> "at 150% volumetric efficiency with some boost that is 238 cfm, so my super-charger goal is 238cfm at +8 to +10 psi boost from e-turbo"

### Power System

The electrical power budget for the compressor:

- **Required power**: ~700 watts (theoretical, assuming reasonable efficiency)
- **Supply voltage**: 48V
- **Regulator engagement RPM**: 4,000 RPM (regulator begins supplying 48V above this threshold)
- **Maximum operating RPM**: 12,000 RPM

> "theoretically that requires 700 watts if there aren't major inefficiencies... very possible from a 48v regulator that engages at 4krpm+ and runs well up to 12k rpm"

### Brushless Motor Torque and Current Relationship

The compressor uses a brushless DC motor. A key characteristic noted:

> "compressor-torque is proportional to the current needed in the brushless motor to sustain enough magnetism to provide that torque"

This is the standard torque-current relationship for brushless motors (torque ≈ Kt × I), meaning:
- Higher boost demand = higher compressor torque = higher motor current draw.
- The 700W figure is a peak/near-peak demand estimate at the boost target.
- At lower boost levels, current (and power) draw will be proportionally lower.

### Integration Notes

- The 48V bus is separate from the main 12V vehicle bus and is regulated up from the alternator/battery system.
- The regulator only engages at 4,000+ RPM, meaning the electric compressor provides no boost at idle or very low RPM — appropriate for a supercharger role on a high-revving engine.
- The 12,000 RPM upper limit of the regulator matches high-RPM engine operation where boost demand is greatest.
