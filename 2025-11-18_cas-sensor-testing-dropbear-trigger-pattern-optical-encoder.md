# CAS Sensor Testing with Dropbear, Custom Trigger Wheel Setup, and Optical Encoder Notes

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @__._.alice._.__, @lysdex, @kzairsoft*

## Summary

This thread covers testing a crankshaft angle sensor (CAS) outside the vehicle using the rusEFI Dropbear board, configuring a custom/modified trigger wheel pattern in the software, and notes on the high resolution available from optical encoders as an alternative trigger source. The user is fabricating a custom trigger wheel window design and wants to validate CAS signal reading before committing to metal fabrication.

## Details

### Testing CAS Sensor Outside the Car with Dropbear

@Alice asked whether it is possible to test a CAS (crankshaft angle sensor) outside of the car using the Dropbear board, prior to finalizing a custom trigger wheel design:

> "Hey, im curious if it's possible to test my cas sensor outside of my car very easily with the dropbear. Since i am modifying the windows design, I want to make sure it can actually read it before I have it made from metal"

This is a practical validation step: bench-testing the sensor and signal quality with the rusEFI Dropbear board before committing the wheel design to metal fabrication.

### Trigger Pattern Configuration for Modified Wheel

@Alice confirmed that the trigger pattern for her modified wheel had been set up in the software:

> "i have the trigger pattern set up properly in the software for my modified wheel"

The Speeduino decoders documentation (which covers many of the same trigger pattern types supported by rusEFI) was referenced as a resource:

> https://wiki.speeduino.com/en/decoders

This wiki covers the decoder types and their configuration parameters, which is applicable when defining a custom or non-standard trigger wheel pattern.

### Fuel Pressure Regulator (FPR) Actuation Note

In the same session, @Alice also noted that her FPR was actuating during testing but not reaching the expected target pressure:

> "I tested it and it actuates, but at a different psi"

No resolution was documented in this snippet, but this may indicate an FPR calibration or spring rating mismatch.

### Optical Encoders for High-Resolution Trigger Signals

@BroKingscooters raised the option of using optical encoders as trigger sources:

> "optic encoders can have crazy high res."

Optical encoders can provide significantly higher tooth counts than typical reluctor wheels, which can improve ignition and injection timing resolution. The key requirement is that the trigger windows (slots or teeth) are consistent — consistent windows produce consistent signals, which leads to consistent ECU behavior:

> "if there is consistent windows the signals will be consistent?"

The consistency of the physical trigger pattern directly determines the quality of the decoded signal. This is particularly relevant when designing or modifying a trigger wheel, as irregular window spacing will cause timing errors.
