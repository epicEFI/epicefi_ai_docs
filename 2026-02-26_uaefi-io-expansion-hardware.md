# Expanding I/O on super uaEFI / epicEFI: Pins, MOSFETs, CAN, and Arduino

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @offtheband, @Hydrocarbon, @jeybee, @ggurov*

## Summary

This thread covers a detailed discussion of hardware I/O expansion options for the super uaEFI / epicEFI platform. A user running out of outputs for alternator, fans, AC, boost solenoid, and WMI failsafe control prompted a deep dive into several techniques: using DC2 PWM pins with external MOSFETs, repurposing injector outputs as low-side drivers, using input pins as 3.3V outputs, the mega_epic CAN+Arduino expansion approach, button pin ADC repurposing, and a custom SO-23 MOSFET driver board.

## Details

### The Problem: Running Out of I/O

The super uaEFI has limited spare I/O. A typical build that already consumed spare injector outputs still needed to control:
- Alternator
- Cooling fans
- AC system
- Boost solenoid
- WMI (water-methanol injection) failsafe

### Solution 1: mega_epic Expansion via CAN Bus + Arduino

The recommended approach for adding large amounts of I/O is the **mega_epic** project:
- Uses **CAN bus** to communicate between the rusEFI ECU and an external **Arduino** (or similar microcontroller)
- Adds many additional I/O channels beyond what the base board provides
- Documented in the `#mega_epic` discussion topic in the rusEFI/epicEFI Discord

### Solution 2: DC2 PWM Pins (p16/p18)

If the DC2 output is not in use, its PWM output signals can be accessed on **pins p16 and p18** (labeled `dc2pwm`):
- Output voltage: **3.3V**
- Maximum current: **250 mA**
- Requirement: Must use an external **MOSFET** to switch any meaningful load — the pins alone cannot drive high-current devices directly

### Solution 3: DC2 (+/-) Pins — Cannot Be Repurposed

The DC2 (+/-) pins go directly to the MCU and **cannot be repurposed** for other functions, even though they appear accessible on the schematic. Do not attempt to use these for alternative outputs.

### Solution 4: Injector Outputs as Generic Low-Side Outputs

The injector outputs on the epicEFI board can be used as **generic low-side outputs** (not just for injectors):
- Maximum current: **2 amps**
- No special configuration needed — do not need to be enabled as injectors in the epic firmware
- Driven by **VNLD5090** injector drivers (SOIC package), approximately **$2 each**
- The VNLD5090 requires only a **3.3V or 5V input** and a **pull-down at the gate**
- Described as "a hybrid MOSFET just for injectors" — suitable for any low-side switching up to 2A

### Solution 5: Input Pins Repurposed as 3.3V Outputs

Certain input pins can be used as **low-current 3.3V outputs**:
- This capability was enabled by ggurov in the firmware
- The 3.3V output is accessible at the **back of the voltage divider**, above the MCU
- Useful for logic-level signals or triggering external circuits

### Solution 6: Button Pins as Free ADC Inputs

On the uaEFI, the button pins are implemented as standard **0–5V ADC pins** with a **10k pull-down resistor** added:
- Removing the 10k pull-down resistor converts the button pin to a **free ADC input**
- This gives additional analog inputs if the button functionality is not needed

### Solution 7: Pin Sharing

rusEFI/epicEFI has an option to **share pins** between functions. This can allow a single physical pin to serve multiple roles, further extending the effective I/O count.

### Custom MOSFET Driver Board for High-Current Outputs

For applications requiring more than 2A per channel, a custom MOSFET driver board can be built:
- Board dimensions: **100 x 24 mm**
- Fits up to **8x SO-23 MOSFETs**
- Each MOSFET can handle **>30 amps** when the gate is driven correctly
- Well-suited for fans, fuel pumps, or other high-current automotive loads

### mega_epic ADC Integration Example

An example implementation using mega_epic:
- An **ADC input on a Pro Mini** microcontroller reads a signal
- The Pro Mini communicates via CAN/serial to the **ESP32**
- The ESP32 uses the ADC reading to control **LED on/off state**
- This pattern can be extended to control relays, solenoids, or other outputs
