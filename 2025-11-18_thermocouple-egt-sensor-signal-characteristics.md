# Thermocouple EGT Sensor Signal Characteristics

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat (1356732771325968630)*
*Contributors: @Mitch, @Joesphan*

## Summary

The discussion explored why thermocouple sensors used for exhaust gas temperature (EGT) monitoring produce such a small voltage output — sub-100 mV across the full temperature range — and the physical reasons behind their wire construction and signal behavior.

## Details

### Voltage Output Range

Thermocouples used for EGT measurement spread their entire temperature reading across less than 100 mV. Mitch noted he found it surprising how small this voltage swing is given the wide temperature range EGT sensors must cover:

> "I still find it crazy that thermocouple sensors for like EGT spread their reading over like sub 100mV" — @Mitch

### Signal Generation: Peltier / Seebeck Effect

Joesphan explained that thermocouples generate voltage through the Seebeck effect (closely related to the Peltier effect — the thermoelectric phenomenon where a temperature gradient across a bimetallic junction produces a voltage):

> "So they output some voltage due to a peltier effect" — @Joesphan

### Wire Resistance and Heat Propagation

Mitch asked whether the thin wire used in thermocouple sensors is intentional — specifically whether it serves to block heat from travelling up the wire (and skewing the cold-junction reference), with the side effect of introducing high electrical resistance:

> "Is it because the wire is thin to stop head propagating up the sensor wire, but also creates hella high resistance?" — @Mitch

This is consistent with thermocouple design: thin, high-resistivity alloy wires (commonly Chromel/Alumel for K-type) limit thermal conduction along the wire so the measured junction remains at the target temperature, but the high resistance means the millivolt signal requires a dedicated amplifier (such as an AD8495 or MAX31855) to be usable.

### Interference from Soldering

Joesphan noted that soldering wires on a thermocouple coupler board affected the bimetallic interaction between the thermocouple wires:

> "Apparently soldering the wires messed with how the bimetallic interact with each other" — @Joesphan

This is a known issue: thermocouples rely on the Seebeck voltage generated at a specific junction. Introducing solder at an unintended point creates a secondary junction of different metals, adding an error voltage to the reading. Thermocouple connections should be made with proper thermocouple connectors or welded junctions rather than soldered ones wherever possible.

### Application Context

Joesphan was working with a thermocouple coupler board — likely an amplifier breakout (e.g., MAX31855 or equivalent) — intended for EGT monitoring in an engine management context.
