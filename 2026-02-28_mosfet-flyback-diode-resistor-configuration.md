# MOSFET Flyback Diode and Pull-Down Resistor Configuration

*Source: rusEFI Discord, 2026-02-28 | Channel: 1411925674498719775*
*Contributors: @jeybee, @mnemonik*

## Summary

When building MOSFET output circuits for inductive loads (solenoids, injectors), the correct combination of a signal diode and a pull-down resistor is needed alongside flyback protection to manage voltage spikes and ensure reliable switching.

## Details

The conversation confirmed a standard protection topology for MOSFET-driven inductive loads:

- **Flyback diode**: Placed across the inductive load (freewheeling diode) to clamp the reverse-EMF voltage spike that occurs when the MOSFET switches off. Prevents the spike from exceeding the MOSFET's drain-to-source breakdown voltage (V_DSS).

- **Signal diode + resistor pull-down**: A signal diode (e.g., 1N4148 or similar small-signal type) combined with a resistor forms a pull-down circuit on the gate or output node. This ensures the gate is pulled to a defined logic-low state when the driving signal is absent or floating, preventing spurious turn-on.

The question "@jeybee" raised — "you got the right diodes and resistors on those mosfets?" — and @mnemonik's confirmation of the "signal diode, resistor -> pull down and flybacks" topology indicate these are considered mandatory components for correct MOSFET output stage operation in DIY ECU or expansion board designs.

## Notes

- Signal diodes used in pull-down networks should be fast-recovery types to avoid forward-recovery issues at PWM frequencies.
- Flyback diodes must be rated for the supply voltage and peak current of the load being switched.
- Omitting either component risks MOSFET latch-up, gate floating, or destruction from inductive kickback.
