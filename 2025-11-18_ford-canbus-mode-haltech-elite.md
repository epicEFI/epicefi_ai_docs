# Ford CANbus Mode on Haltech Elite ECUs

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ToyotaTerrorist, @Lysdex, @mke*

## Summary

Discussion confirming that Haltech Elite ECUs support a Ford CANbus compatibility mode that allows plug-and-play communication with Ford Falcon BCM (Body Control Module) systems. This mode is saved as a protocol preset in the Haltech software and is available on any Haltech Elite unit, not restricted to specific plug-and-play models. The Ford CANbus protocol data itself is not exposed to users — Haltech keeps this proprietary.

## Details

### Ford CANbus Mode Availability

Any Haltech Elite ECU can be configured for Ford CANbus mode:

> "Any haltech alive can be set up into Ford CANbus mode" — @ToyotaTerrorist

> "correct, they have the protocol saved and easy" — @Lysdex

This mode handles the BCM authentication CAN messages automatically, enabling the Ford Falcon's BCM to recognise the aftermarket ECU and unlock the starter circuit and radio without any additional scripting.

### Verifying the Setting

To confirm the Ford CANbus mode option is present in the software:
- Download the Haltech Elite software (free download, does not require a connected ECU to browse settings).
- Open the software and look for the Ford CANbus protocol option in the CAN configuration section.
- If the setting is present in the downloaded software, it is a real firmware feature and not hardware-specific.

As @mke noted: "Thinking...you'd see that in the software if it's a real setting. Maybe download and confirm and if so I'll see about getting access to the unit."

@ToyotaTerrorist confirmed: "I have confirmed it's in the firmware. They don't however let you look at the physical CANbus data."

### ECU Models Referenced

- Haltech Elite Pro Plug-In ECU: https://www.haltech.com/product/ht-154000-elite-pro-plug-in-ecu/
- Specifically discussed: Elite 1500 (154-pin connector variant)

### Limitations

- Haltech does not publish or expose the underlying CAN message data used for Ford BCM authentication.
- Tunes and firmware are most likely tied to the ECU's serial number (similar to Motec and AEM behaviour).
- Third-party ECUs (including rusEFI/epicEFI) would need to reverse-engineer the protocol independently via CAN sniffing.

### Context

This discussion arose from ToyotaTerrorist's project to run a Ford Falcon Barra engine on a Proteus/epicEFI ECU while keeping the stock BCM functional. The Haltech Elite was being examined as a reference for how the Ford CANbus mode works, with the intent of replicating the handshake in rusEFI Lua scripting.
