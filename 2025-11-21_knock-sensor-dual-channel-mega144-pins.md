# Dual-Channel Knock Sensor Configuration on Mega144

*Source: rusEFI Discord, 2025-11-21 | Channel: epicEFI dev/support*
*Contributors: ggurov, sansvirus.*

## Summary

The rusEFI firmware supports dual-channel knock detection on the mega144 board. The knock sensor pins are hardcoded in the firmware (not runtime-configurable), so users must verify the pin mapping in the firmware source before wiring. The analog knock circuitry must be built before firmware configuration is meaningful; the reference design can be borrowed from the Proteus board schematics.

## Details

**Firmware pin definitions (mega144):**

```c
// knock 1 - pin PF4
#define KNOCK_PIN_CH1 Gpio::F8

// knock 2 - pin PF5
#define KNOCK_HAS_CH2 true
#define KNOCK_PIN_CH2 Gpio::F7
```

Note: The comments say PF4/PF5 but the actual GPIO constants are `Gpio::F8` (CH1) and `Gpio::F7` (CH2). This discrepancy was acknowledged by ggurov directly: `F4 / F7 ^ PF8 = ch1, PF7 = CH2`. Verify against your specific firmware build.

**Hardware requirements:**

- The analog knock signal conditioning circuit must be present on the board before the firmware can detect knock. This is not something that can be enabled purely in software without the supporting circuitry.
- For the 100-pin board, adding knock requires an external addon circuit. The 144-pin brain board has more native options.
- Recommended reference: copy the knock circuit design from the Proteus board schematics.

**Board recommendations:**

- If building a new PnP or brain board that will use knock sensing, prefer the 144-pin variant. It offers more hardware options for knock than the 100-pin board.
- For 100-pin PnP boards, an external addon board providing the analog knock conditioning circuit is the practical path.

**Configuration workflow:**

1. Build the analog knock circuit per the Proteus reference design.
2. Wire knock sensors to the correct physical pins (verify firmware GPIO mapping for your build).
3. The firmware pin assignments are compile-time constants — check with the development team on the exact pins for your specific firmware version before wiring.
4. Submit your MSQ configuration for review once the pinout is finalized.
