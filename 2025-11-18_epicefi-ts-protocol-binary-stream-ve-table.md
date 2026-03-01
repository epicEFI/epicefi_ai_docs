# epicEFI/rusEFI Tuning Software Communication Protocol: Binary Stream, Packet Format, and VE Table Config

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ggurov, @mitch0s*

## Summary

This thread documents the communication protocol between the epicEFI/rusEFI ECU and the tuning software, covering the binary stream data transmission format, the msEnvelope_1.0 packet serial configuration, VE table and bin array definitions, output channel definitions, and the resulting per-frame data size. The serial throughput limitation was identified as being on the tuning-software side rather than in the ECU hardware.

## Details

### Serial Limitation Identified on Tuning-Software Side

@mitch0s confirmed that the serial communication bottleneck is in the tuning software's implementation for reading serial data, not in the ECU hardware itself:

> "honestly I think it should be fine for now. At least I know now the serial limitation is actually on the tuning-software side"

The RP2040/RP2350's USB controller can communicate over "serial" at up to 12 Mbps — more than sufficient. The pico hardware can send messages very rapidly; the constraint is the software reading loop on the tuning-software side.

### Multiple Values for the Same Sensor Type

A proposed protocol feature would allow the ECU to send multiple values of the same type (e.g. voltages for all sensors in one packet). The proposed packet format for multivalued sensor data uses the following structure:

```
[MULTIVALUE] [TYPE_SENSOR_ID] [ATTR_SENSOR_ID] S1:V1 S2:V2 S3:V3 ...
```

- `[MULTIVALUE]` — marker indicating multiple values in the message
- `[TYPE_SENSOR_ID]` — the sensor type identifier
- `[ATTR_SENSOR_ID]` — an attribute/qualifier for the sensor ID
- `S1:V1 S2:V2 S3:V3` — individual sensor IDs and their corresponding values

This allows fetching voltages for all sensors in a single packet rather than issuing separate requests per sensor.

### Binary Stream Transmission

All data from the ECU is packed into a binary stream before transmission:

> "they pack it all into binary stream, and send it as binary"

### Packet Serial Format (msEnvelope_1.0)

The `[Constants]` section configuration for the packet serial format with CRC:

```ini
[Constants]
; new packet serial format with CRC
    messageEnvelopeFormat = msEnvelope_1.0

    endianness            = little
    nPages                = 1

    pageIdentifier        = "\x00\x00"
```

- Uses the MegaSquirt Envelope 1.0 format
- Little-endian byte order
- Single page configuration
- Page identifier is two null bytes `\x00\x00`

### VE Table and Bin Array Definitions

The VE (Volumetric Efficiency) table and associated bin arrays are defined as:

```ini
veTable     = array, U16, 20372, [32x32], "%", 0.1, 0, 0, 999, 1
veLoadBins  = array, U16, 22420, [32], {bitStringValue(fuelUnits, fuelAlgorithm) }, 1, 0, 0, 650, 0
veRpmBins   = array, U16, 22484, [32], "RPM", 1, 0, 0, 18000, 0
```

- `veTable`: 32x32 array of U16 values at offset 20372, units `%`, scale factor 0.1, range 0–999
- `veLoadBins`: 32-element U16 array at offset 22420; units are dynamic via `bitStringValue(fuelUnits, fuelAlgorithm)`, range 0–650
- `veRpmBins`: 32-element U16 array at offset 22484, units RPM, range 0–18000

### Output Channels and wallFuel Variable

Output channels are configured through the rusEFI/epicEFI user interface. An example output channel definition:

```ini
wallFuel = scalar, F32, 1716, "", 1, 0
; total TS size = 1720
```

- `wallFuel` is a 32-bit float scalar at offset 1716
- The total TunerStudio (TS) page size for output channels is **1720 bytes**

### CAN/Serial Frame Size

Each communication frame is approximately:

- **1720 bytes** of payload data
- Plus ~10 bytes of overhead for request/response framing
- **Total: approximately 1730 bytes per frame**

> "1720 bytes, plus some tax on the request/response bits, 1730 bytes per frame"
