# epicEFI Serial Communication Protocol over USB (CRC Mode)

*Source: rusEFI Discord, 2026-02-20 | Channel: 1356732771325968630*
*Contributors: @emiougus, @ggurov, @SanGawku*

## Summary

A user integrating with epicEFI over USB found that only the `S` (signature) command returned a response; the `O` (output channel block) and `A` (burst mode) commands produced no output. The resolution is that epicEFI requires CRC mode to be activated before those commands will respond. The activation sequence, CRC frame format, and the INI-defined nature of most commands were documented in this thread.

## Details

### Problem

@emiougus was sending serial commands to epicEFI over USB:
- `S` — firmware signature — worked and returned a response.
- `O` (with 2+ additional bytes per the INI definition) — no response.
- `A` (burst mode) — sent no bytes in response.

### Root Cause

epicEFI requires **CRC mode to be enabled** before responding to commands like `O` and `A`. Simply sending the command byte without first entering CRC mode results in silence.

### Command Reference

Many epicEFI serial commands are not fixed — they are defined in the `.ini` configuration file that ships with TunerStudio. The only universally defined, non-INI commands are:
- `S` — firmware signature (works without CRC mode)
- `F` — initiate CRC mode handshake (see below)
- `k` — check whether CRC mode is currently active (does NOT enable it)
- `Q` / `q` — alternative commands; behavior is implementation-specific, may or may not return data

The auto-detect routine in TunerStudio uses one standard configuration for device detection.

### CRC Mode Activation Sequence

Provided by @SanGawku (also defined in the INI as `#define TS_CRC_CHECK_COMMAND`):

1. **Send `F`** — the ECU should respond with `001`.
2. **Send `S`** — perform signature check; confirms identity of the connected ECU.
3. After this handshake, **all subsequent commands must be wrapped in CRC frames**.

### CRC Frame Format

```
[2-byte length] [payload bytes] [4-byte CRC32 checksum]
```

- **Length field**: 2 bytes, big-endian, indicates the byte count of the payload.
- **Payload**: the command and any parameters (e.g., `O` followed by offset and length bytes).
- **CRC32**: 4-byte CRC32 checksum over the payload.

### Output Channel Command (`O`) Parameter Detail

The `O` command takes **4 additional bytes** (not 2 as initially assumed from the INI):
- Bytes 1-2: **offset** into the output channel block
- Bytes 3-4: **length** of data to read

This must be sent as a complete CRC frame after CRC mode is activated.

### Notes

- The `k` command checks if CRC is already active but does not enable it — use `F` to enable.
- The repository contains additional protocol documentation. @ggurov offered GitHub org access to @emiougus (GitHub: `Emiougus`) to review the internal docs.
- `A` (burst mode) also requires CRC mode to be active before it will transmit data.
