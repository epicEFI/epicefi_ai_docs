# 8HP EEPROM: Locating the IMMO/VIN Block and Extracting EEPROM Data

*Source: rusEFI Discord, 2025-11-23 | Channel: 1411925674498719775*
*Contributors: fast335xi (@fast335xi), Tera (@teraflopping)*

## Summary

How to locate the immobilizer (IMMO) block in the 8HP EEPROM by searching for the VIN, and important notes about bench mode file extraction: BMD/BM3 bench mode reads generate a file rather than pulling a true full dump, and the EEPROM is typically stored as a separate smaller file. The `dump_eeprom` command can be used within rusEFI tools to extract just the EEPROM portion.

## Details

### Locating the IMMO Block in the 8HP EEPROM

To find the immobilizer data in an 8HP EEPROM dump:

1. Search the binary for the VIN string.
2. Once the VIN is located, the entire immobilizer block spans **all data above (prior to) the VIN**, up to and including the lines that end with `00 00` sequences.
3. That contiguous chunk — from the start of the zeroed-out region down to and including the VIN — constitutes the full IMMO block.

This approach lets you identify and isolate IMMO-related data without needing a map or offset table for a specific firmware version.

### Bench Mode File Extraction Caveats

When files are pulled using bench mode (BMD / BM3):

- The resulting file **does not contain the VIN**. Bench mode downloads generate a synthesized file rather than a raw physical dump of flash/EEPROM.
- BMD/BM3 tools typically **generate** a file rather than pulling an actual full binary from the ECU.
- The EEPROM is usually stored as a **separate, smaller file** alongside the main flash image — it is not embedded in the main bench-mode output file.

**Implication**: If you need the IMMO block (which includes the VIN), a bench mode file alone is insufficient. You need to pull the separate EEPROM file or use a full read method that captures EEPROM.

### Extracting EEPROM from rusEFI

Within rusEFI software, use the `dump_eeprom` command to extract only the EEPROM data. This is useful when you want to save or inspect the EEPROM contents without a full flash read:

```
dump_eeprom
```

This command dumps just the EEPROM segment, which can then be inspected for IMMO/VIN content or transferred as needed.
