# TunerStudio Logging: Data Rate, Bluetooth Issues, and CAN Bus Logging

*Source: rusEFI Discord, 2026-02-22 | Channel: 1356732771325968630*
*Contributors: @Bloopdog, @Joesphan, @Neos6443*

## Summary

A user was experiencing inconsistent logs — pulls would drop off early, skip RPM ranges (e.g., jumping from 4k to 7k), or show a ghost needle value suggesting incomplete data capture. The root cause was a combination of using Bluetooth (too slow/lossy for high-rate logging) and having the data logging rate set to "fastest" rather than a locked value. Recommended fix: use a USB cable and lock the logging rate to ~250–500 samples/sec. The session also touched on CAN bus as a viable alternative logging transport.

## Details

### Symptoms

- Logs only capture ~1 out of 5 pulls to redline
- Log traces drop off at ~5000 RPM even when the engine revved to 7000+
- RPM trace skips large ranges (e.g., jumps from 4k to 7k with no data in between)
- Analog gauges show a ghost peak reading after the pull, at 4–5k, indicating the log stopped capturing before the run was complete
- TunerStudio visibly freezes for a moment during these drop-offs

### Why Logged RPM Cannot Be Wrong

Joesphan clarified an important point: the logged RPM is the same value the ECU uses to fire fuel and spark. If the log shows 5000 RPM, the engine was running at 5000 RPM at that moment from the ECU's perspective. A discrepancy between the logged RPM and a dashboard tachometer would indicate a tach output calibration issue, not a logging error.

### Root Cause 1: Bluetooth Connection

- Bluetooth is inherently slow and lossy for high-frequency data streaming
- It is unsuitable for capturing full-throttle pulls and high-RPM data at any meaningful sample rate
- Joesphan: "you understand how bastardized and slow bt is right" / "just get rid of bt its useless / too slow for dash"
- Neos6443: "BT is slow and probably lossy. Use a cable."
- BT may be acceptable for light monitoring once a tune is set, but not for data logging pulls

### Root Cause 2: "Fastest" Data Rate Setting

- The "fastest" setting in TunerStudio sends data as fast as the link allows, without a fixed rate
- This can overwhelm the connection (especially BT) and cause the freeze/stutter behaviour
- Fix: **lock the logging rate to a fixed value**
  - Over USB cable: ~250–500 samples/sec is recommended; Joesphan suggested "lock it to like 250" and "cable you can lock it at 500 it'll probably be fine"
  - Over Bluetooth: Joesphan suggested as low as ~10 samples/sec

### Recommended Setup for Logging Pulls

1. Use a **USB cable** (not Bluetooth)
2. Set the TunerStudio logging rate to a **locked value** (250–500 samples/sec over USB)
3. Use Bluetooth only for casual monitoring (idle checks, live gauges) with a low rate setting

### CAN Bus as Alternative Logging Transport

Joesphan noted that the CAN bus implementation is now "real good" and capable of supporting a logger over CAN:
- CAN bus can handle ~3000 packets/sec
- A CAN-connected logger device (e.g., a Raspberry Pi or similar) could be used as a dedicated data logger
- This requires a separate logger solution — epicEFI provides the CAN output, but you need an external device to record it
- Neos6443 asked about CAN > DoIP or UDS/CCP for logging as potential future directions
