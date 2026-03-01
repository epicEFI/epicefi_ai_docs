# rusEFI H7 Firmware: Memory Misalignment Crashes and GDB Debugging on Windows

*Source: rusEFI Discord, 2025-11-23 | Channel: 1401895238481744052*
*Contributors: ggurov (@ggurov), mynameisdeleted (@mynameisdeleted)*

## Summary

A known intermittent crash was identified on the STM32H7-based rusEFI ECU caused by memory misalignment in core rusEFI. The crash was reproducible on the default tune and was accompanied by stuttering behavior (time jumps in output channels) after multiple tune writes, and an unexpected uptime reset. The debugging workflow involved attaching GDB over a COM port using the `arm-none-eabi-gdb` toolchain. The leading hypothesis was a memory out-of-bounds or overflow condition in a tune table, and the fix path involved isolating whether the bug was tune-specific or firmware-specific by cross-flashing tunes between setups.

## Details

### Symptom: Memory Misalignment on H7

The core rusEFI codebase has a known memory misalignment issue specific to the H7 chipset that surfaces intermittently. Symptoms include:

- **Crash / hard fault** — occurs on the H7 and was reproducible even with the default (stock) tune, not just custom tunes.
- **Stutter after multiple writes** — after writing the tune several times, a stutter becomes apparent where `seconds` and time counters jump relative to each other.
- **Uptime reset** — the ECU resets its uptime counter because all output structures are zeroed out on the crash/restart path.

### Root Cause Investigation

Two leading theories were discussed:

1. **Memory out-of-bounds or uninitialized variable access** in a tune table — `mynameisdeleted` noted that "memory out-of-bounds or uninitialized [variables]" are a common cause of this class of fault.
2. **Bounds check or overflow in a tune table** — suspected to be a table index exceeding its allocated buffer.

The crash was confirmed reproducible on the default tune, which ruled out tune-specific configuration as the sole cause and pointed toward a firmware-level issue.

### Debugging Approach

**Step 1 — Isolate tune vs. firmware:**
- Cross-flash tunes between setups: flash the affected tune on a different firmware build and vice versa.
- Goal: narrow down whether the crash is triggered by a specific tune content or by a firmware code path.

**Step 2 — Reset to stock firmware:**
- Perform a full reset from stock on the H7 to eliminate any configuration state.

**Step 3 — Attach GDB debugger:**

On Windows, using the ARM GDB toolchain:

```
E:\debug> bin\arm-none-eabi-gdb.exe

(gdb) file rusefi.elf
(gdb) target extended-remote \\.\com35
(gdb) monitor auto_scan
(gdb) attach 1
```

- `file rusefi.elf` — loads the debug symbols from the ELF build artifact.
- `target extended-remote \\.\com35` — connects to the ECU over COM35 (Black Magic Probe or equivalent SWD/JTAG debugger on that COM port). Adjust the COM port number to match your system.
- `monitor auto_scan` — instructs the probe to scan and identify the target MCU.
- `attach 1` — attaches to thread 1 (the main/only thread in the bare-metal context).

After attaching, standard GDB commands (`bt`, `info registers`, `x/20x $sp`, etc.) can be used to inspect the crash state.

### Adding Tune Variables and Output Channels

When extending rusEFI to add new tunable parameters:

- **`rusefi_config.txt`** — add variable definitions here to include them in the tune structure that is saved in ECU memory (flash/EEPROM).
- **`output_channels.txt`** — add entries here for any values that should be sent out to TunerStudio (TS) as live output channels (gauges, logging fields, etc.).

These two files must be kept in sync when adding a new tunable feature that also needs real-time monitoring in TS.

### Notes

- The stutter/time-jump symptom after multiple writes may indicate a progressive memory state corruption that only manifests after several write cycles.
- The simulator was considered as an alternative to hardware debugging to reproduce the crash, but it was unclear whether the H7-specific misalignment would surface in a simulator environment.
