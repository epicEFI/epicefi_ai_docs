# GPS Data Transmission to epicEFI via CAN Bus

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), AetherVape (@aethervape)*

## Summary

GPS data from a USB-connected GPS module (e.g., Adafruit Ultimate GPS USB) can be forwarded to epicEFI over CAN bus using a Raspberry Pi 5. The approach uses shell scripting with the `cansend` utility to read GPS NMEA output and transmit it as CAN frames. Combined with the ADS1115 ADC setup, the Pi can handle both ADC channels and GPS data over a single CAN connection.

## Details

**Hardware:**
- GPS module: Adafruit Ultimate GPS USB (connects to Raspberry Pi 5 via USB-C controller board)
- CAN interface: 2-channel isolated CAN HAT with MCP2515 on the Raspberry Pi 5
- The same CAN HAT used for ADC (ADS1115) can also carry GPS CAN frames

**Software approach on the Raspberry Pi:**
- Use `cansend` command-line utility to inject CAN frames onto the bus
- Write a bash script that:
  1. Reads GPS NMEA sentences from the USB GPS device (e.g., `/dev/ttyUSB0`)
  2. Parses relevant fields (position, speed, etc.)
  3. Formats the values into CAN frame payloads
  4. Calls `cansend` to transmit each frame to epicEFI

**Example conceptual flow:**
```bash
# Read GPS, extract data, send via cansend
# GPS device typically outputs NMEA to a serial/USB device node
cansend can0 <frame_id>#<data_bytes>
```

**Combined ADC + GPS over CAN:**
- AetherVape confirmed the architecture: ADS1115 ADC data and GPS data both routed through the MCP2515 CAN HAT to epicEFI
- ggurov confirmed this is feasible: "read adc, do some calcs, send it via that usb-canbus thing"
- The Pi running TSDash can serve as a combined data concentrator (GPS + ADC) feeding epicEFI over CAN

**Requirements:**
- Custom CAN message format/IDs must be defined and configured in epicEFI to receive and interpret the GPS and ADC frames
- `can-utils` package on the Pi (provides `cansend`, `candump`, etc.)
- GPS device must be accessible as a serial port on the Pi
