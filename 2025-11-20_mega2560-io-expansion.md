# Mega2560 as I/O Expansion Module: ADC, Buttons, Outputs, VSS, GPS, USB HID

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov (@ggurov), Tera (@teraflopping)*

## Summary

An Arduino Mega2560-based board is used as a peripheral I/O expander connected to the epicEFI ECU. It adds analog inputs, digital buttons, on/off outputs, and PWM outputs, as well as quad wheel speed sensor inputs, GPS NMEA parsing, and USB keyboard input bridged over CAN bus.

## Details

**Current I/O expansion capabilities (Mega2560 expander):**
- +16 additional ADC (analog) input channels
- +16 additional digital button inputs
- +8 additional on/off digital outputs
- +11 additional PWM hardware outputs (in progress at time of discussion)

**Quad VSS (Vehicle Speed Sensors):**
- Four-wheel individual speed sensor inputs are supported on the expander.
- This feeds into the 4-wheel traction control system (see related traction control doc).

**GPS / NMEA parsing:**
- ggurov is actively working on NMEA sentence parsing from a GPS unit.
- GPS is connected to one of the expander's serial ports.
- Parsed GPS data (position, speed) will be available as ECU channels.

**USB HID keyboard input over CAN bus:**
- USB HID (keyboard/keypad) devices can be connected and their input is bridged to the ECU over CAN bus.
- Project repo: https://github.com/ggurov/USB_HID_CAN_BRIDGE
- Uses an ESP32-S3-USB-OTG board (official Espressif dev board with USB OTG support) as the bridge.
- Enables use of inexpensive off-the-shelf USB keypads as button inputs to the ECU.

**CAN button listeners:**
- CAN bus button inputs are supported via a listener interface.
- FOME firmware contributed an implementation; epicEFI includes this.
- The CANchecked CAN switch panel (https://canchecked.com.au/?page_id=528) is a compatible device.
- All ECU variables are accessible over CAN via RPC; any config variable can be written over CAN; some output channels are writable.
- This allows external CAN devices (button panels, keypads, data loggers) to both read and control ECU state without Lua scripting.

**Build environment:**
- The epicEFI build container is available at: https://github.com/ggurov/rusefibuildcontainer
- Supports building epicEFI firmware (and potentially the GDI8 codebase).
- Build time on ggurov's machine: ~53 seconds.
- Workflow: WSL2 + Ubuntu LTS + Docker on Windows is supported and confirmed working.
- Cursor IDE is recommended; the repo includes AI context files so Cursor can explain the codebase automatically.
