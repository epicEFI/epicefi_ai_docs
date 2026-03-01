# TunerStudio CAN Virtual Input Pins, INI Layout, and Serial Protocol Reference

*Source: rusEFI Discord, 2025-11-24 | Channel: tuning-and-software (1356732771325968630)*
*Contributors: ggurov (@ggurov), Tera (@teraflopping)*

## Summary

Discussion covering how CAN Virtual Input Pins are configured in TunerStudio INI files, how to extend or redesign those menus, the TunerStudio plugin API, and the epicEFI/rusEFI serial protocol for anyone wanting to build an alternative tuning interface. Includes the actual INI entry points for the CAN bus menu and virtual input dialog, the serial protocol specification links, and the runtime data frame parameters (200–250 Hz, 2700 bytes per frame).

## Details

### CAN Virtual Input Pins in TunerStudio INI

The CAN Virtual Input Pins UI (the dialog where you assign CAN address, byte, and bit to virtual inputs) is defined in `tunerstudio.template.ini`.

**Menu entry point:**
```ini
menu = "CAN Bus"@@if_ts_show_top_level_can_menu
    subMenu = canBusMain,       "CAN Bus Settings"@@if_ts_show_top_level_can_menu
    subMenu = std_separator@@if_ts_show_top_level_can_menu
    subMenu = canVirtualInputs, "CAN Virtual Input Pins"
```

**Dialog definition:**
```ini
dialog = canVirtualInputs, "CAN Virtual Input Pins", xAxis
    panel = canVirtualInputs_id
    panel = canVirtualInputs_byte
    panel = canVirtualInputs_bit
    panel = canVirtualInputs_timeout
    panel = canVirtualInputs_default
```

The key identifier is `canVirtualInputs`. To add columns or redesign the layout, modify the `dialog` block. TunerStudio supports multi-column layouts (e.g., `xAxis` for horizontal arrangement) and can embed gauge panels alongside table inputs.

### TunerStudio Layout Control

Within a dialog definition, reasonable layout control is available:
- 3-column layouts are supported.
- Gauges can be embedded alongside input panels.
- The `@@if_ts_show_*` preprocessor conditionals control visibility based on firmware-defined flags.

### TunerStudio Plugin API

TunerStudio (TS) exposes a **plugin API** for extending functionality. The API is Java-based (TS is a Java application). ggurov noted it is *"kind of weird — real weird"* but functional. The plugin API allows external code to interact with TS at runtime.

For anyone wanting to avoid writing Java directly, ggurov proposed a MITM (man-in-the-middle) architecture:
- The **rusEFI console** connects to the ECU over USB.
- The console creates a listener socket that TS connects to as if it were the ECU.
- A separate application connects to the console's listener interface and implements its own UI or data processing.
- This allows modern-language tooling (C#, Python, etc.) to consume ECU data while TS continues to handle configuration.

### epicEFI / rusEFI Serial Protocol

The ECU serial protocol is documented and implementable independently of TunerStudio:

- **Frame format:** action letter + 2 bytes length + payload + 2 bytes CRC16
- **Update rate:** 200–250 Hz
- **Frame size:** ~2700 bytes per frame (full output channel snapshot)

Reference documents:
- ECU definition file format: `https://www.efianalytics.com/TunerStudio/docs/EFI%20Analytics%20ECU%20Definition%20files.pdf`
- Megasquirt serial protocol (basis for rusEFI): `https://www.msextra.com/doc/pdf/Megasquirt_Serial_Protocol-2014-10-28.pdf`

### ECU Sensor Wiring — Backfeed Prevention

Related item from the same conversation: **always power sensors from the same relay/switched line that powers the ECU**.

If a sensor is wired to a always-on power supply but its signal pin connects to the ECU (which is switched), the sensor can back-feed voltage through the ECU's internal signal-conditioning circuits and keep the ECU partially powered after key-off. This can cause issues including ECU drain, unexplained wake events, and potential damage.

Sensor wiring rules per ggurov:
- **MAP, IAT, CLT, TPS** — powered directly from the ECU's internal 5V supply. Safe, no external power needed.
- **Hall effect cam/crank sensors** — typically sink the ECU's pullup current (no external power output). Wire their power supply from the same ignition relay as the ECU.
- **NTC thermistors (water temp, air temp)** — one wire to ground, other wire to ECU analog input. No external power needed; the ECU provides the reference voltage.
- **12V injectors and coils** — power from the ECU main relay; do not use a separate always-on supply.

### Notes on Building a Replacement Tuning Console

ggurov described the effort level for a full TS replacement as very large: *"it's a huge project... can't really do it slowly, you yourself lose context."*

The existing rusEFI console (Java) already implements part of the required functionality and can serve as the backend proxy. Building a fresh UI layer that communicates through the console's interface would be more feasible than a full rewrite.
