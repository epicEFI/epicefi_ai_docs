# epicEFI and EpicTuner: Project Overview and Ecosystem

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @sangawku, @joesphan*

## Summary

A newcomer to the rusEFI/epicEFI ecosystem asked for a rundown of the projects, hardware, and tools. SanGawku (Devin) provided authoritative answers clarifying the relationships between epicEFI firmware, rusEFI, epicECU hardware, and EpicTuner. Key point: epicEFI is a rusEFI fork that runs on most rusEFI-compatible ECUs, not exclusively epicECU hardware.

## Details

### epicEFI Firmware

- epicEFI is a **fork of rusEFI**
- Runs on **most, if not all, rusEFI-compatible ECUs** — not exclusive to epicECU hardware
- Known deployments by community members:
  - All uaefi boards (SanGawku runs epicEFI on all of his uaefi units)
  - VatoTuned OBD1 plug-and-play Honda ECU
- The purple board variant mentioned in this session was running epicEFI

### EpicTuner

- Developed by SanGawku (Devin / @sangawku)
- Intended as a **modern replacement for TunerStudio**
- Supports **epic, rusEFI, and fome** firmware variants
- Built-in features not found in TunerStudio:
  - Integrated Lua script editor
  - Serial/USB console
  - Engine sniffer (live data visualization)
- EpicTuner enhancements apply to both rusEFI and epicEFI firmware, not just epicEFI

### epicECU Hardware

- Designed by Joesphan (@joesphan)
- Was in beta/sold-out status at time of this conversation — no production stock available
- V1 hardware was being refined; early units were beta releases
- Conceptually: a purpose-built ECU for running epicEFI, but the firmware itself is not locked to this hardware

### Project Structure Summary

| Component | Who | Notes |
|-----------|-----|-------|
| rusEFI firmware | rusEFI community | Base/upstream firmware |
| epicEFI firmware | Fork of rusEFI | Runs on most rusEFI ECUs |
| epicECU hardware | Joesphan | Dedicated hardware for epicEFI; beta/limited stock |
| EpicTuner | SanGawku | TunerStudio replacement; supports epic/rus/fome |
| uaefi boards | Various | Community ECU hardware; runs rusEFI or epicEFI |

### Use Cases Mentioned

- Micro sprints / motorcycle engines at high RPM (17,000 RPM GSXR 600 example)
- OBD1 Honda PNP installations
- Kit/kit-car injection systems (lower cost alternative to commercial ECUs like AEM Infinity or ECUMaster EMU Pro)
