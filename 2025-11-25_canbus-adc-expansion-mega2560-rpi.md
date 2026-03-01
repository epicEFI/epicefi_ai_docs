# CAN Bus ADC Expansion: Mega2560/MCP2515 and Raspberry Pi Options

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), AetherVape (@aethervape)*

## Summary

Adding additional ADC (analog-to-digital converter) channels to epicEFI via CAN bus is a well-supported pattern. Two hardware paths were discussed: a Mega2560 + MCP2515 setup that can deliver 16 ADC channels over CAN, and a Raspberry Pi 5 based alternative using a 2-channel isolated CAN HAT (MCP2515) with an ADS1115 ADC. The Raspberry Pi path requires writing custom CAN interaction code for epicEFI.

## Details

**Path 1: Arduino Mega2560 + MCP2515 (recommended for simplicity)**
- Hardware: Arduino Mega2560 microcontroller + MCP2515 CAN controller
- Capability: Up to 16 ADC channels delivered over CAN bus to epicEFI
- The Mega2560 reads its onboard ADC inputs and transmits the values as CAN frames
- DIY Speeduino kits (which include a Mega2560) can be used as the basis for experimentation
- This is the established approach with known working patterns in the community

**Path 2: Raspberry Pi 5 + MCP2515 CAN HAT + ADS1115 (backup / alternative)**
- Hardware: Raspberry Pi 5 + 2-channel isolated CAN HAT (contains MCP2515) + ADS1115 ADC module
- ADS1115: 4-channel 16-bit I2C ADC; provides additional analog inputs to the Pi
- The Pi can run TSDash simultaneously while handling CAN communication
- MCP2515 on the CAN HAT handles the physical CAN bus interface
- Workflow: Pi reads ADS1115 ADC values → performs any necessary calculations → sends results via CAN to epicEFI
- **Caveat:** Requires writing custom CAN bus interaction code for epicEFI integration (no out-of-the-box support)

**Pin access note:** ggurov mentioned that certain pins on the ECU may be accessible electronically and could be added to the pin selection menu in the firmware — AetherVape agreed to investigate which pins are reachable.

**Architecture summary:**
```
ADS1115 (I2C) --> Raspberry Pi 5 --> MCP2515 CAN HAT --> CAN bus --> epicEFI
Arduino Mega2560 ADC --> MCP2515 (SPI) --> CAN bus --> epicEFI
```

**For GPS data alongside ADC:** See related doc on GPS-over-CAN integration.
