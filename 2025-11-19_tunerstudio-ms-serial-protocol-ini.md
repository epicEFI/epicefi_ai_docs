# TunerStudio / Megasquirt Serial Protocol and INI File Format

*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @ggurov, @mitch0s*

## Summary

A session where @ggurov walked @mitch0s through how the Megasquirt Serial Protocol works and how the TunerStudio `.ini` configuration file describes the ECU's memory layout and communication commands. The discussion covered protocol framing, page-based memory organization, the burn/write command structure, and scalar data type definitions as used in rusEFI's INI files.

## Details

### Reference Document

- Megasquirt Serial Protocol specification (2014): https://www.msextra.com/doc/pdf/Megasquirt_Serial_Protocol-2014-10-28.pdf
- TL;DR of the protocol: packet type byte ("command"), followed by 2-byte length, followed by 2-byte CRC16

### TunerStudio INI File Structure

#### Connection / Identity Section

```ini
[TunerStudio]
    queryCommand    = "S"
    versionInfo     = "V"  ; firmware version for title bar
```

- `S` command: TS sends this to query the ECU identity/signature
- `V` command: TS sends this to retrieve the firmware version string, displayed in the title bar

#### Constants Section (Memory Layout)

```ini
[Constants]
    messageEnvelopeFormat = msEnvelope_1.0   ; new packet serial format with CRC

    endianness            = little
    nPages                = 2

    ; Note endian in pageIdentifier! second page is 0x0100
    pageIdentifier        = "\x00\x00", "\x00\x01"

    ; Page sizes in bytes
    pageSize              = 49020, 256

    pageReadCommand       = "R%2i%2o%2c", "R%2i%2o%2c"
    burnCommand           = "B%2i", ""
    pageValueWrite        = "C%2i%2o%2c%v", "C%2i%2o%2c%v"
    pageChunkWrite        = "C%2i%2o%2c%v", "C%2i%2o%2c%v"
    crc32CheckCommand     = "k%2i%2o%2c", "k%2i%2o%2c"
    retrieveConfigError   = "e"
```

- Pages define logical sections of ECU memory exposed to TunerStudio
- rusEFI's current tune is approximately 49KB (page 0: 49020 bytes, page 1: 256 bytes)
- `%2i` = 2-byte page index, `%2o` = 2-byte offset, `%2c` = 2-byte count, `%v` = value data
- `burnCommand` writes the RAM values to flash/EEPROM

#### Scalar Data Type Definition

Format: `name = scalar, TYPE, OFFSET, "units", scale, offset, min, max, digits`

- **TYPE** codes:
  - `S08` = signed 8-bit integer
  - `U08` = unsigned 8-bit integer
  - `U16` = unsigned 16-bit integer
  - `F32` = 32-bit float
- **OFFSET** = byte offset within the page
- **scale** = multiplier applied to raw value
- **offset** = additive offset applied to raw value (after scaling)

Example scalar definitions from rusEFI INI:

```ini
launchRpmLockAdder         = scalar, S08, 8,     "rpm",   20.0, 0, -2500, 2500, 0
rpmHardLimit               = scalar, U16, 10,    "rpm",    1.0, 0,     0, 20000, 0
engineSnifferRpmThreshold  = scalar, U16, 12,    "RPM",    1.0, 0,     0, 30000, 0
multisparkMaxRpm           = scalar, U08, 14,    "rpm",   50.0, 0,     0,  3000, 0
maxAcRpm                   = scalar, U08, 15,    "rpm",   50.0, 0,     0, 10000, 0
maxAcTps                   = scalar, U08, 16,    "%",      1.0, 0,     0,   100, 0
maxAcClt                   = scalar, U08, 17,    "deg C",  1.0, 0,     0,   250, 0
compressionRatio           = scalar, U08, 18,    "CR",     0.1, 0,     0,    25, 1

; CAN wheel speed sensor scaling factors (F32 at high offsets in page 0)
canWheelRLFactor           = scalar, F32, 48996, "#",      1.0, 0,     0,   100, 3
canWheelRRFactor           = scalar, F32, 49000, "#",      1.0, 0,     0,   100, 3
canWheelFLOffset           = scalar, F32, 49004, "#",      1.0, 0,     0,   100, 3
canWheelFROffset           = scalar, F32, 49008, "#",      1.0, 0,     0,   100, 3
canWheelRLOffset           = scalar, F32, 49012, "#",      1.0, 0,     0,   100, 3
canWheelRROffset           = scalar, F32, 49016, "#",      1.0, 0,     0,   100, 3
```

### Developer Notes

- @mitch0s noted that the current approach in custom ECU software requires writing explicit getter/setter methods on both the tuning software side and the ECU firmware side, which is laborious
- The preferred future direction is an XML or similar declarative config that both sides can parse to automatically generate the communication layer from class/field type information
- rusEFI's INI-based approach achieves a similar goal with plain-text declarations that TunerStudio interprets directly
