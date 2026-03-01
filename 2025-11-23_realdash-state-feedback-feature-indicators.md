# RealDash State-Based Button Colors and Feature Status Feedback

*Source: rusEFI Discord, 2025-11-23 | Channel: 1430309988688990298*
*Contributors: ggurov (@ggurov), Walter R. (@walterronny), Aboy (@aboy)*

## Summary

Discussion of how to provide visual feedback in RealDash (the Android/iOS dashboard app) when rusEFI/epicEFI features such as anti-lag or launch control are active or inactive. Two approaches were explored: reading a state variable from the ECU into RealDash to drive conditional button colors, and using existing output signals (check engine light, tachometer) as indirect status indicators when RealDash's CAN variable read capability is limited.

## Details

### Context: Controlling Features via RealDash

Aboy demonstrated the ability to **send CAN requests from RealDash to the ECU** to activate features (e.g., enabling a specific ETB map, toggling anti-lag or launch control). The question raised was how to get visual confirmation back in RealDash that the feature had actually been activated on the ECU side.

### Proposed Approach 1: State Variable via CAN Read

Walter R. asked whether it is possible to read a state variable from the ECU back into RealDash over CAN to drive conditional button behavior:

- **State 0**: Feature is off — button displays in grey.
- **State 1**: Feature is on — button displays in another color (e.g., green or red).

ggurov's response to the question *"can you read a variable from realdash?"* was left open-ended, indicating this may be possible depending on how the RealDash CAN descriptor XML is configured, but was not confirmed as straightforwardly supported for arbitrary user variables at the time of this conversation.

### Proposed Approach 2: Check Engine Light Blink Codes

ggurov suggested using the **check engine light (CEL) output** as a binary indicator for feature states, since it is easy to wire and visible on most dashboards:

- Flash the CEL once = anti-lag active.
- Flash the CEL twice = launch control active.
- This approach was agreed to be suitable for simple binary features (on/off).
- Aboy noted it may **not be suitable for ETB map selection** (where there are more than two states) but is good for anti-lag and launch control.

### Proposed Approach 3: Tachometer Blip for Map Indication

For features with multiple states (e.g., ETB map 1, 2, 3), ggurov proposed:

- **Blip the tachometer** to a specific RPM that encodes the active map number.
- Example: blip to 1000 RPM = map 1, blip to 2000 RPM = map 2, etc.
- Aboy confirmed this idea positively (*"Nice"*).
- ggurov noted this feature would require firmware development and would not be immediately available: *"might be a few [weeks] before I can get that done"*.

### Implementation Status (as of 2025-11-23)

- Sending CAN control commands from RealDash to rusEFI/epicEFI: **working** (Aboy demonstrated).
- Reading arbitrary ECU state variables back into RealDash for button color changes: **under investigation**.
- CEL blink code for feature status: **proposed**, not yet implemented in firmware.
- Tachometer blip for map indication: **proposed**, planned for future firmware work.

### Practical Notes

- RealDash communicates with rusEFI/epicEFI over CAN using the rusEFI CAN descriptor XML file. Custom output channels can be mapped to RealDash gauges and buttons if the CAN descriptor is extended to include them.
- The check engine light approach requires the CEL output pin to be software-controllable independent of actual fault status — this is supported in rusEFI/epicEFI via scripting or a dedicated output channel.
