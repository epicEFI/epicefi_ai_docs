# CAN Button Box: Map Switching, DFCO, and Runtime ECU Control

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ggurov, @Lysdex*

## Summary

@ggurov demonstrated a CAN-connected button box that integrates with epicEFI/rusEFI to provide physical controls for runtime engine management functions. The button box communicates over CAN bus and can trigger any software-accessible function in the firmware. Features implemented in ggurov's fork include multiple ETB (electronic throttle body) pedal-to-throttle maps switchable on the fly, fuel trim adjustments, DFCO enable/disable, launch control activation, and SD card logging controls. This represents the broader capability of rusEFI to accept CAN inputs for live ECU control — not just passively reading sensors.

## Details

### Button Box Overview

Reference project: https://github.com/ggurov/canButtonBox

The button box connects to the ECU via CAN bus (SocketCAN / canButtonBox protocol). Physical controls on the box include switches and rotary knobs.

### Implemented Features in ggurov's Fork

The following features are accessible via the CAN button box in the epicEFI firmware:

**Map Switching (ETB Pedal Maps):**
- 4 selectable ETB pedal-to-throttle maps
- Designed for switching between driving modes: valet, normal, hot, SPICY
- Rotary knob input to cycle between maps

**Fuel Trim on the Fly:**
- Twisty knobs to add or remove fuel in real time
- Reset to base fuel trim available

**ETB Map Adjustment:**
- Rotary control to shift the active ETB map up or down
- Reset to default

**DFCO (Decel Fuel Cut-Off) Control:**
- Enable or disable DFCO from the button box
- Console command equivalent: `enable/disable DFCO` (accessible from the rusEFI console)

**Launch Control:**
- Button to activate/deactivate launch control
- Combined with noise (anti-lag / exhaust effects)

**SD Card Data Logging:**
- "Start SD log" button
- "Rotate SD log" (start a new log segment) button

### Extensibility

> "it's really anything we can dream up in the software" — @ggurov

Because the button box simply sends CAN messages that the firmware maps to internal function calls, any feature that is software-accessible can in principle be wired to a physical button. The open-source nature of the firmware means custom CAN inputs can be added relatively easily.

@Lysdex noted: "open source pull and change"

### DFCO Configuration

DFCO (Decel Fuel Cut-Off) is the feature that cuts injector pulse during overrun/deceleration. In rusEFI/epicEFI it is found in the fuel cut configuration section of the tuning software and can also be toggled at runtime via the button box or console.

Console command to enable verbose CAN (related, for diagnostics):
```
set verbosecan on
```

### Context: Four ETB Maps for Mode Selection

@ggurov described the intended use case for the four ETB pedal maps:

> "4x ETB pedal to ETB maps — so switch between those, valet, normal, hot, SPICY — add/remove fuel on the fly — turn on launch/make noise — enable/disable DFCO" — @ggurov

This allows a single ECU to support significantly different driving behaviours without re-tuning, simply by selecting a different pedal map and fuel trim combination from the button box.
