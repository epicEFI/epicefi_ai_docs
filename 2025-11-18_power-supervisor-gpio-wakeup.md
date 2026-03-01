# Power Supervisor: GPIO Pin Count, Voltage Input, Wake-Up Circuit, and Current Limits

*Source: rusEFI Discord, 2025-11-18 | Channel: epicEFI-hardware (1430309988688990298)*
*Contributors: @joesphan, @paleppp*

## Summary

A discussion about building or replacing a power supervisor module for an epicEFI installation explored running the supervisor function without a dedicated Arduino, using a device with 30 GPIO pins and 12 VDC direct power input. The conversation covered how to implement an ignition-based wake-up signal using a GPIO pull-up resistor, and established that total current draw from the supervisor GPIO outputs must remain under 50 mA.

## Details

### Arduino-Free Power Supervisor

The standard approach involves an Arduino as the power supervisor. A user proposed replacing it with a device that provides:

- **30 GPIO pins**
- **12 VDC power input** (direct automotive voltage, no 5 V regulator required from a host board)

The conclusion was to evaluate whether the wake-up requirement could be met before committing to the Arduino-free design.

### Wake-Up Circuit via Ignition Switch

The critical question for an Arduino-free supervisor is how to implement a hardware wake-up signal so the ECU powers up when the ignition is turned on.

Proposed implementation:
1. Configure a GPIO pin with an **internal or external pull-up resistor** (pin held HIGH at rest).
2. Route the ignition switch to **pull the GPIO pin to GND** when the ignition is turned on.
3. The falling edge (HIGH → LOW) on the GPIO triggers the wake-up routine.
4. If needed, the ignition switch wiring can be modified to provide this ground-switched signal.

An alternative polarity (pull-down with ignition supplying V+) is also viable depending on the device's GPIO configuration options.

### Current Draw Limit

Total current draw across all GPIO outputs on the power supervisor must remain **below 50 mA**. This constraint applies to the aggregate of all connected loads on the supervisor's GPIO outputs, not per-pin. Individual GPIO pin ratings must also be respected per the specific device's datasheet.
