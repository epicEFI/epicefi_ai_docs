# NTC Thermistor Pull-Up Resistor Requirement

*Source: rusEFI Discord, 2026-02-28 | Channel: 1411925674498719775*
*Contributors: @Bearded_Bogan, @Joesphan*

## Summary

All NTC (negative temperature coefficient) thermistors used as temperature sensors — including coolant temperature (CLT), oil temperature, and intake air temperature (IAT) sensors — require a pull-up resistor to a reference voltage. Without a pull-up, the ECU's ADC cannot read a meaningful voltage from the sensor. This requirement is not optional and applies universally to NTC sensor circuits.

## Details

### Why a Pull-Up Is Required

An NTC thermistor is a passive resistive element: it changes resistance with temperature but produces no voltage on its own. To convert that resistance to a voltage the ECU's analog-to-digital converter can measure, the thermistor must form a voltage divider with a pull-up resistor connected to a known reference (typically 5V). As temperature changes, the thermistor resistance changes, and the divider output voltage changes proportionally.

Without a pull-up resistor the signal pin floats or reads the full reference voltage regardless of temperature — either way, the ECU sees no useful signal.

@Joesphan summarized this clearly: "That's how you convert resistance to voltage which the ADC can read."

### Application: Dual-Function Sensor with Shared Connector

@Bearded_Bogan was installing a dual-function sensor (combined oil temperature and oil pressure in one body) and was unsure whether the pull-up resistor was needed for the temperature circuit of this type of sensor. The answer is yes: regardless of whether the sensor also contains an independent pressure element (which outputs a ratiometric voltage of its own), the NTC thermistor portion of any dual sensor still needs a pull-up on the temperature signal wire.

### Confirming the Pull-Up Is Present

If a temperature sensor is wired correctly and a calibration is loaded, but the ECU still shows no reading or an obviously wrong fixed value, the absence of a pull-up resistor is one of the first things to check. Symptoms of a missing pull-up:

- Signal pin reads 0V or full reference voltage regardless of actual temperature.
- ECU software shows an out-of-range or pegged temperature value.

### Wiring Notes

When reterminating harness connectors for a sensor, @Bearded_Bogan noted using opposing-keyed (polarized) connectors so the sensor connector physically cannot be inserted incorrectly. Heat-shrinking the assembled connector prevents accidental shorts at the terminals. These practices are straightforward but worth documenting for new builders.

## Notes

- The specific pull-up resistor value must match the thermistor calibration table in the ECU software. Commonly used values in aftermarket ECUs include 2.49 kΩ (Speeduino convention) and 2.2 kΩ. rusEFI supports custom thermistor tables so any pull-up value can be accommodated.
- If the pull-up is built into the ECU hardware (as it is on some rusEFI boards for specific inputs), adding an external pull-up in parallel will change the effective resistance and corrupt the calibration. Always verify whether the input has an internal pull-up before adding one externally.
