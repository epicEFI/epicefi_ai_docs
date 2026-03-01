# CAN Bus RPM Signal Parsing and DBC Format

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ggurov, @YeOldePirate*

## Summary

Discussion covering how to read RPM from a CAN message on ID 0x201 (decimal 513), including the correct byte extraction method and the corresponding DBC (Vector CAN database) format definition. The RPM value is encoded as a 16-bit little-endian integer occupying the first two bytes of the message payload.

## Details

### CAN Message Layout for RPM

- **CAN ID:** 0x201 (decimal 513)
- **Signal:** RPM
- **Encoding:** First two bytes of the payload, little-endian (Intel byte order)
- **Width:** 16 bits
- **Scale:** 1 (raw value = RPM directly)

### DBC Format Definition

The signal in Vector DBC format:

```
BO_ 513 BASE1: 8 Vector__XXX
 SG_ RPM : 0|16@1+ (1,0) [0|0] "RPM" Vector__XXX
```

Field breakdown:
- `0` — start bit position (bit 0 of the payload)
- `16` — signal length in bits
- `@1+` — little-endian (Intel) byte order, unsigned
- `(1,0)` — scale factor 1, offset 0
- `[0|0]` — min/max (not specified)

### Practical Extraction

To read RPM from CAN ID 0x201:
1. Receive the full 8-byte frame for ID 0x201.
2. Extract bytes 0 and 1 (the first two bytes) as a 16-bit little-endian unsigned integer.
3. The resulting integer is the RPM value directly (no scaling needed).

Note: reading only byte 0 alone will give only the low byte of the RPM value and will be incorrect for RPM values above 255. The full two bytes must be combined.

### Common Confusion

A user initially asked if "byte 1" alone would give the RPM. The clarification was that RPM occupies two bytes — reading only the first byte is insufficient for RPM values exceeding 255 RPM. The correct approach is to read both bytes as a 16-bit little-endian word.
