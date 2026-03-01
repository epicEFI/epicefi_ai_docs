# epicEFI RP2040 Firmware Rewrite in C++ — Architecture and Design Decisions

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @Mitch, @ggurov*

## Summary

Developer @Mitch (mitch0s) described an ongoing ground-up rewrite of a custom ECU firmware targeting the RP2040/RP2350 microcontroller, now compiled as C++ rather than C. The move to C++ unlocks use of the standard library (vectors, structs with methods), enabling a cleaner serial IO buffer, JSON-based config storage with LittleFS, a non-linear tuning table implementation, and a software-based ignition/injection scheduler using microsecond-epoch trigger ranges. The architecture uses both RP2040 cores — the main core handles output triggering and serial IO, while the second core handles sensor reads and table indexing.

## Details

### Why C++

Rewriting the firmware to compile as C++ was motivated by the ability to use `std::vector` and other standard library containers. Key benefit: vectors allow a dynamically-sized output buffer for serial communication, which prevents the ECU from momentarily freezing while transmitting large messages (e.g. full table data). Without a buffer, the transmit of large messages blocked execution.

### Serial IO Buffer Design

- **Output side:** Uses a `std::vector`-backed queue. `serialWriterTask()` pulls and sends one byte from the buffer per call, distributing the write cost across many loop iterations rather than blocking.
- **Input side:** Does not require a large buffer — messages are handled one at a time and the buffer is cleared after processing.
- ggurov noted the RP2040 USB virtual COM (VCOM) is capable of up to 12 Mbit (low-speed USB) and suggested exploring multi-stream VCOM to overcome serial bandwidth limits; the current rusEFI implementation achieves ~6 Mbit over VCOM.

### Configuration Storage (JSON + LittleFS)

- Configuration is stored as **individual JSON files** on the RP2040's 2 MB flash, one file per configuration object (e.g. one file per injector instance, one per table).
- **Filesystem:** LittleFS — slightly slower than SPIFFS but designed to handle power-loss safely (no partial writes/corruption).
- **Load performance:** Loading an `Injector` class from JSON takes ~1 ms including parse time. This is acceptable; behavior with large 3D tables (256–1024 values) was flagged as a future performance concern.
- **Startup timing:** ggurov set the target as config load completing within 50 ms of power-on. Spending several seconds loading configs at startup would be problematic.
- Once loaded from disk, each configuration element is held in memory as a native struct (not kept as parsed JSON).
- Individual files mean that saving a change to one injector instance only requires re-serializing that one file, not all instances of the same type.

### Table (3D Map) Architecture

- 3D tables have two axes (bins) and a flat data array.
- **Axis bins are non-linear** (arbitrary breakpoints, not evenly spaced).
- Data is stored flat: `index = (y * table.width) + x`, which is faster than a 2D array index.
- **Load percentage** = absolute percentage of the current value relative to the maximum bin value for that axis. This is the input/lookup value for the axis.
- **Cursor percentage** = where the cursor should be displayed in the tuning UI (derived from bin position, adjusted for non-linearity).
- Helper functions and operator overloads handle table lookup; the operator`[]` is overloaded for convenience.

### Ignition and Injection Scheduling

The scheduling system does not use hardware timers. Instead:

1. Each `Injector` and `Ignition` instance holds a `trigger_range` consisting of two microsecond epoch values: `{start_epoch, stop_epoch}`.
2. The main loop continuously checks the current system epoch. If the current time is within the `trigger_range` of an output, that output is activated; it is deactivated when outside the range.
3. The main loop runs with a cycle time in the low single-digit microseconds (typically sub-5 µs), giving fine-grained timing resolution.

### TDC Event and Timing Prediction

- Crank decoders are called by interrupt on each tooth signal.
- The decoder determines whether TDC occurred at or just before the current tooth. If TDC occurred between teeth, the decoder back-calculates the epoch at which TDC actually occurred.
- When TDC is detected, the decoder calls `TDCEvent()`.
- `TDCEvent()` uses the crank cycle duration, engine acceleration/deceleration, and the variance between the previous predicted TDC and actual TDC to estimate the epoch of the **next** TDC (360 degrees ahead).
- Ignition and injection `trigger_range` values are then calculated as offsets relative to this predicted next TDC epoch.

### Multi-Encoder Support

The decoder architecture supports different crank encoder/trigger wheel types. Each decoder is a separate module that implements the tooth-counting and TDC-detection logic specific to that wheel pattern, then calls the common `TDCEvent()` function. A decoder for the Honda G4ED trigger pattern was in development but untested at the time of the discussion.

### Dual-Core Task Distribution (RP2040)

- **Core 0 (main core):** Output triggering (injector/ignition), serial IO. Loop time in the ones of microseconds. Time-critical.
- **Core 1 (second core):** Table indexing (map lookups), sensor reads, and other non-time-critical firmware tasks.

Separating triggering onto its own dedicated core ensures output timing is not affected by the variable latency of sensor processing or table lookups.

### rusEFI Codebase Context

ggurov noted that the rusEFI codebase itself is heavily C++ with approximately 900 unit tests exercising calculations on mock data, specifically to guard the trigger event parsing and math subsystems against regressions.
