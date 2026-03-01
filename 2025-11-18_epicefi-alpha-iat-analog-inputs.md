# epicEFI Alpha — IAT Monitoring and Analog Input Limitations

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @Joesphan*

## Summary

@Joesphan reported running out of analog inputs on the epicEFI alpha hardware while trying to add IAT (Intake Air Temperature) monitoring. The upcoming revised epicEFI board is planned to include provisions for IAT. The epicEFI platform supports a maximum of 8 analog inputs. An additional issue was noted with VW stock IAT NTC sensors reading inaccurately at low temperatures due to very high resistance values.

## Details

### Analog Input Limit

The epicEFI system supports a maximum of **8 analog inputs**. The alpha version of the hardware was fully consumed by other sensors, leaving no free analog input for IAT.

### IAT in New epicEFI Revision

The next version of the epicEFI board (post-alpha) will include dedicated hardware provisions for IAT monitoring, addressing the limitation encountered on the alpha board.

### VW Stock IAT NTC Sensor Issue

A stock VW IAT NTC thermistor was found to read inaccurately at low temperatures. The cause is that NTC (Negative Temperature Coefficient) thermistors have very high resistance values at low temperatures — the resistance increases as temperature drops. If the ADC input circuit is not designed for the high-resistance range, or if the lookup table does not cover the high-resistance end, the reading will be out-of-range or inaccurate at cold temperatures.

### Workaround Options (for alpha hardware)

- Use **digital inputs** for some measurements where a threshold (on/off) is sufficient rather than a continuous analog value.
- Add an **external ADC** (Analog-to-Digital Converter) to expand the number of available analog channels beyond what the microcontroller provides natively.
