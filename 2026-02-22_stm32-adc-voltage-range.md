# STM32 ADC Voltage Range, 5V Tolerance, and Firmware Scaling

*Source: rusEFI Discord, 2026-02-22 | Channel: 1401895238481744052*
*Contributors: @ggurov (@ggurov), @MykkH (@mykkh)*

## Summary

A discussion about analog sensor signal voltage ranges on STM32-based epicEFI/rusEFI hardware clarified the relationship between the STM32's digital 5V tolerance, the ADC's actual input voltage range (limited by Vdda), and how the firmware handles signals from sensors that output 0-5V. The STM32 GPIO pins can tolerate 5V signals without damage, but the ADC converter itself is limited by Vdda (1.62-3.6V maximum). Standard epicEFI hardware includes a voltage divider that scales 0-5V sensor inputs down to the 0-3.3V ADC range; firmware then scales the raw ADC reading back to represent the full 0-5V range.

## Details

### STM32 5V Tolerance (Digital GPIO)

- STM32 microcontrollers used in epicEFI hardware are **5V tolerant on digital GPIO pins**
- A 5V signal will not damage the STM32 pin itself
- This applies to digital inputs (trigger signals, switches, etc.)

### ADC Input Voltage Limit (Vdda)

Despite 5V tolerance on digital pins, the **analog-to-digital converter (ADC) input range is strictly limited by Vdda**:

- Vdda (analog supply voltage) range: **1.62V to 3.6V maximum**
- ADC input voltage must not exceed Vdda
- Typical operating Vdda on rusEFI/epicEFI hardware: **3.3V**
- Effective ADC input range: **0V to 3.3V**

Applying more than 3.6V directly to an ADC input pin risks damage to the analog front-end, even if the digital GPIO path is 5V tolerant.

### Hardware Voltage Divider for 5V Sensors

Standard epicEFI board designs include analog input conditioning circuitry (voltage dividers / resistor networks) that attenuate 0-5V sensor signals down to the 0-3.3V ADC input range:

- TPS (Throttle Position Sensor): 0-5V sensor output → divided to 0-3.3V at ADC pin
- MAP (Manifold Absolute Pressure) sensor: same treatment
- Any 0-5V sensor: same treatment

The division ratio scales the full 5V range to fit within the ADC's safe input range.

### Firmware Scaling Back to 0-5V

The firmware compensates for the hardware voltage division by applying a corresponding scale factor to the raw ADC reading:

- Raw ADC reading range: 0 to 3.3V (or the equivalent counts)
- Firmware scaling: multiplies or maps the reading back to represent 0-5V
- Result: sensor values displayed in TunerStudio and used for calculations reflect the actual 0-5V physical sensor output

This means a TPS reading of "5V = WOT" is correctly reported even though the ADC pin never sees more than ~3.3V.

### Caution for Non-Standard Boards

On boards **without** proper voltage divider conditioning (e.g., bare development boards like Arduino Mega Giga R1 without analog input protection):

- Connecting a 5V sensor directly to an ADC pin may saturate or damage the converter
- The Giga R1 was flagged during this discussion as potentially lacking opamps or dividers for analog inputs
- Users should verify their specific board's analog input conditioning before connecting 5V sensors

### Summary Table

| Parameter | Value |
|-----------|-------|
| STM32 digital GPIO 5V tolerance | Yes |
| ADC Vdda max | 3.6V |
| Typical ADC input range (epicEFI hw) | 0-3.3V |
| 5V sensor input handling | Voltage divider on board |
| Firmware compensation | Scales 0-3.3V ADC reading back to 0-5V |
