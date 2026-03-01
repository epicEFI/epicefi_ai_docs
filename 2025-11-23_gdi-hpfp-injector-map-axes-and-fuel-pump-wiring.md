# GDI/HPFP: Injector 3D Map Axes, HPFP Delay Map, and Fuel Pump Channel Usage

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: Joesphan (@joesphan), fast335xi (@fast335xi), ggurov (@ggurov)*

## Summary

Discussion clarifying the axis definitions for the GDI injector fuel mass table, the HPFP delay time map based on battery voltage, and whether the HPFP signal is timed to crank/cam angles. Also notes that an injector channel can be used to power a fuel pump in a pinch.

## Details

**Injector 3D fuel table axis definitions:**

The fuel mass table used for GDI injector control is a 3D map with the following axes:

- **X axis:** RPM
- **Y axis:** Fuel mass (target fuel delivery)
- **Z axis:** Rail pressure target (in PSI)

This was confirmed by fast335xi in response to a question about the X axis assignment.

**Injector pressure-to-flow rate relationship:**

The relationship between rail pressure and injector flow rate is not a simple formula that is easily defined in tuning software. It is stored in the injector EEPROM (as part of the injector's factory calibration data). In the ECU's fuel model, a multiplier is applied to account for pressure-dependent flow rate variation. Users cannot directly edit this as a standalone table — it comes from the injector's own characterization data.

**HPFP delay time map:**

The High Pressure Fuel Pump (HPFP) has a delay time map that varies as a function of battery voltage. Lower battery voltage requires different timing to ensure the solenoid actuates correctly, since solenoid response time is affected by the available drive voltage. An image of this map was shared (Discord attachment: `image.png` in channel 1356732771325968630, message 1441946659834695761).

**HPFP signal timing — crank/cam vs. demand-based:**

ggurov asked whether the HPFP solenoid signal is timed relative to crank or cam angles. fast335xi clarified that for a triple-cam engine setup (e.g., BMW N54/N55 class), the HPFP is driven in a more demand-based mode — the system always has pressure and the pump responds to fuel demand rather than being strictly synchronized to a specific crank/cam event. This is engine architecture dependent.

**Using an injector channel to power a fuel pump:**

Joesphan asked whether an injector output channel was used to power a fuel pump. This is a confirmed viable approach in epicEFI — an injector driver channel can be repurposed to switch the fuel pump relay or directly drive a low-side pump load, using the injector channel's switching capability outside of its normal injection function.

**GDI8 oscilloscope safety note (related context):**

When probing the GDI8 board's H-bridge power stage with a mains-powered oscilloscope (e.g., Rigol), the scope's ground prong connects its probe ground clips to earth. If the scope's power supply ground is also connected to the GDI8 board, attaching the probe ground strap to the high-side FET source (which is floating at bus voltage) will short it. Solutions:

- Remove the third (ground) prong from the scope's AC cord to isolate it from earth.
- Use a battery-powered or USB-isolated scope with the laptop disconnected from the wall.
- Use a car battery as the GDI8 power source instead of a mains supply — this breaks the common ground path.
