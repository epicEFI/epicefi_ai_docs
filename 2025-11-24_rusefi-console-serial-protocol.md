# rusEFI Console Serial Protocol and Architecture

*Source: rusEFI Discord, 2025-11-24 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Tera (@teraflopping)*

## Summary

Technical description of the binary serial protocol used between the rusEFI console (and TunerStudio) and the ECU, including packet structure, data rates, and frame size. Also covers the architecture for how an alternative or extended console application could interface with the ECU alongside TunerStudio via a man-in-the-middle (MITM) approach.

## Details

### Protocol Structure

The rusEFI/TunerStudio ECU serial protocol uses a simple binary framing format:

```
[Action byte (1)] [Length (2 bytes)] [Payload] [CRC16 (2 bytes)]
```

- **Action byte:** Single character identifying the command or response type.
- **Length:** 2 bytes (big- or little-endian per the Megasquirt protocol convention).
- **Payload:** Variable-length data.
- **CRC16:** 2-byte checksum over the payload.

Reference documents:
- ECU definition format: EFI Analytics ECU Definition Files spec (TunerStudio `.ini` format)
- Serial protocol: Megasquirt Serial Protocol (2014-10-28 revision) — `Megasquirt_Serial_Protocol-2014-10-28.pdf`

### Data Rates and Frame Size

- Nominal refresh rate: **200–250 Hz**
- Output frame size: **2700 bytes per frame**
- Transport: **USB** (rusEFI console to ECU)

At 250 Hz × 2700 bytes = ~675 kB/s of output data from the ECU. This is the full output channel data block.

### Console Architecture — MITM Approach

The existing rusEFI console is a Java application approximately 25 years old. Its core protocol interface is well-understood and could be extended or proxied by a new application without replacing TunerStudio entirely.

Proposed architecture for extending the console:

1. The **rusEFI console** connects to the ECU over USB as normal.
2. The console also starts a **local listener** that TunerStudio connects to as if it were the ECU (MITM / proxy).
3. A **separate modern application** (written in any language/framework) connects to the same listener.
4. The listener multiplexes data between TunerStudio and the new application.

This approach preserves TunerStudio compatibility (needed for Linux/Mac users and for the full INI-based configuration UI) while allowing a modern interface to operate alongside it at higher UI refresh rates or with additional features (e.g., CAN bus sniffer, custom dashboards).

The TunerStudio console UI layout is controlled by scripted INI definition files, making the UI partially declarative but limited in flexibility.
