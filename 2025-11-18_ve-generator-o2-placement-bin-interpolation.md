# VE Generator Prerequisites, O2 Sensor Placement, and Bin Interpolation

*Source: rusEFI Discord, 2025-11-18 | Channel: 1392172039430738111*
*Contributors: @ggurov, @MykkH, @DCwerx*

## Summary

Three related tuning topics from the same session: the prerequisites for using the VE (Volumetric Efficiency) Generator in rusEFI/epicEFI, what VE actually means, the correct approach when an O2 sensor is placed too close to an oversized exhaust exit (causing false lean readings at idle), and how to interpolate bins in the tuning software.

## Details

### VE Generator Prerequisites

The VE Generator in rusEFI/epicEFI requires that certain calibration values are correctly set before it will produce accurate results:

> "the ve generator assumes you have your deadtimes and injector sizes correctly"
> — @ggurov

Prerequisites before running the VE Generator:
- **Injector deadtimes**: must be accurately characterized for the injectors installed
- **Injector size/flow rate**: must be entered correctly

If either of these is wrong, the VE Generator output will be offset and require manual correction afterward.

### Definition of VE (Volumetric Efficiency)

> "VE is mechanical volumetric efficiency"
> — @ggurov

Volumetric efficiency describes how effectively the engine fills its cylinders with air relative to the theoretical maximum. A VE table in the ECU maps RPM and load (typically MAP or TPS) to a percentage representing this efficiency, which is used to calculate the required fuel mass.

### O2 Sensor Placement Near an Oversized Exhaust Exit

When an O2 sensor is mounted close to an oversized exhaust pipe exit (or a collector that is significantly larger than the primary pipes), atmospheric oxygen can be drawn back into the exhaust stream at low flow velocities (idle, low RPM):

> "With the O2 so close to the oversized exhaust exit I would expect to be mixing atmosphere O2 with the exhaust at lower exhaust flow velocity. So I would find the VE the engine runs/idles best with and not focus on O2 during those conditions"
> — @MykkH

> "Yep"
> — @DCwerx

**Recommendation**: At idle and low RPM with a poorly placed wideband sensor, ignore the O2 reading and instead use the VE table approach — tune the VE table values empirically to find the fueling at which the engine idles and runs smoothest, rather than chasing a specific lambda/AFR target that may be corrupted by atmospheric oxygen ingestion.

This is a hardware placement issue; the correct fix is to relocate the O2 sensor upstream, but the VE-based workaround is valid for getting a running tune.

### Interpolating Bins in TunerStudio / EpicTuner

To interpolate between two known values in a table (e.g., VE table or fuel table):

> "you can select the bottom thing, and right click on it to interpolate the bins"
> — @ggurov

Steps:
1. Select the cells or axis range you want to interpolate
2. Right-click on the selection
3. Choose the interpolate option from the context menu

This linearly fills in the intermediate values between the endpoints, which is useful for smoothing tables after making spot corrections.
