# Serial Communication: Sensor Data Output Frequency, USB vs UART, and Python GUI

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat (1356732771325968630)*
*Contributors: @ggurov, @Mitch*

## Summary

Mitch implemented per-sensor output frequency control in the epicEFI tuning software serial pipeline. The discussion covered two design approaches for sensor data delivery, the practical performance limits of serial-over-USB on the RP2040, the ability to expose serial throughput statistics as a firmware variable, and a Python GUI threading issue caused by the global interpreter lock (GIL).

## Details

### Sensor Data Output Frequency Design

Mitch added support for using a sensor as a table axis input and then evaluated two approaches for delivering sensor data to the tuning software:

**Option 1 — Per-sensor frequency control (preferred):**
Each sensor can be configured to output at its own rate. Examples:
- RPM: 15 Hz
- Coolant temperature: 5 Hz
- Other sensors can have individually tailored update rates

The firmware also tracks which sensors the tuning software is actively monitoring, so only relevant data is transmitted.

**Option 2 — On-demand polling (rejected as wasteful):**
The tuning software requests specific sensor values; the ECU responds individually. Mitch considers this less efficient compared to Option 1.

> "I can change the frequency of which each sensor outputs (e.g. 15hz for rpm, 5hz for coolant temp) and stuff like that. Also add a way for the firmware to track what sensor's are being tracked by the tuning software. The other option instead of tracking is just making the tuning software request value, and ECU sends it back. But that's pretty wasteful imo." — @Mitch

### Multi-Sensor Performance Benchmark

In testing, the system handled 5 OBD sensors all running at 20 Hz without problems. Mitch planned to implement further changes only if capacity became an issue.

> "Seems to handle 5 sensors at 20hz fine, I'll implement some changes if it ever becomes an issue tbh" — @Mitch

### Serial Throughput Statistics as a Variable

ggurov asked whether it would be possible to collect statistics on bytes transmitted through the serial pipe and expose them as a firmware variable. Mitch confirmed this is feasible:

> "it being serial, can you just hack together some stats for bytes through pipe and ... output that as a variable?" — @ggurov
>
> "yeah 100%" — @Mitch

### USB Serial Speed Limitations on RP2040

Mitch identified that the serial communication speed over USB from the RP2040 is limited by how the RP2040 implements USB serial. A faster alternative is to use UART with a UART-to-serial converter:

> "I think it's a limitation with how rp2040 does serial communications over the USB. I would be able to get it faster using uart and uart-to-serial converter thing" — @Mitch

### Serial Read Function and Python GIL Issue

Mitch was implementing a serial read function on the tuning software side (written in Python). When the sleep delay in the serial polling loop was reduced to increase responsiveness, the GUI became sluggish — resizing windows and other UI actions were affected. This is attributed to Python's global interpreter lock (GIL), which prevents true parallel execution of Python threads:

> "Only issue now is that when I lower that sleep delay it wants to slow down the GUI, like resizing and stuff. Probably something to do with Python's global interpreter lock" — @Mitch

This is a known Python limitation: aggressive polling in one thread starves the GUI event loop thread. Potential mitigations include using `multiprocessing` instead of `threading`, offloading serial I/O to a subprocess, or using asyncio.
