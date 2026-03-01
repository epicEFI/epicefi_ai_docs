# Runtime Data Rate and CAN Bus Telemetry Throughput

*Source: rusEFI Discord, 2025-11-21 | Channel: general-tuning (1401895238481744052)*

*Contributors: ggurov, SeV, SanGawku*

## Summary

rusEFI's runtime data rate to TunerStudio is capped at approximately **333 Hz** on the STM32F4/F7 hardware, while fome achieves up to **500 Hz** on the same hardware. epicEFI has not yet addressed this specific cap but has active work on CAN bus packet accounting and telemetry bandwidth management. CAN bus comfortably handles up to ~2,000 packets/second; the system simultaneously supports multiple broadcast streams (extended broadcast, rusEFI broadcast, Haltech, mega_epic, CAN button box).

## Details

### Runtime Data Rate Cap

- rusEFI is reported to be **capped at ~333 Hz** for the runtime data stream to TunerStudio/tuning software.
- fome (a rusEFI fork) reportedly achieves **500 Hz** on identical STM32 hardware.
- SeV: *"BTW were you able to fix runtime data rate in epic? Rusefi is capped at 333 Hz for some reason while fome can do 500 Hz on the same hardware"*
- ggurov: *"haven't looked"* — this had not been addressed in epicEFI at the time of discussion.

### CAN Bus Packet Accounting

- ggurov implemented **CAN bus packet accounting** to track packets per second on the bus.
- Observed baseline: ~100 packets/second at idle; peaks up to ~300 packets/second when ADC values are actively changing.
- SanGawku noted that Honda UDS-based systems can achieve ~150 packets/second with UDS overhead, suggesting 300 pps is competitive.
- ggurov stated the **comfortable CAN bus limit is ~2,000 packets/second**, giving significant headroom over current usage.

### Simultaneous CAN Broadcast Streams

- epicEFI supports multiple simultaneous CAN broadcast protocols:
  - Extended broadcast (epicEFI native)
  - rusEFI broadcast (compatibility)
  - Haltech broadcast (compatibility)
  - mega_epic (Arduino Mega satellite module)
  - CAN button box
- ggurov confirmed all five streams active together and observed *"lots of headroom"*.

### USB TX/RX Monitoring

- USB TX/RX packet accounting had been implemented prior to this session.
- Combined with CAN packet accounting, this gives visibility into total communication bandwidth usage.

### Design Philosophy

- ggurov: *"trying to be easy on the can bus, have to balance how much data is sent and how often"*
- The goal is sufficient update rate for tuning and datalogging without saturating the bus or CPU.
