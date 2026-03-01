# epicEFI Decoder Architecture and TDCEvent() Timing System

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @Mitch (mitch0s), @ggurov*

## Summary

This thread documents the core decoder and event-scheduling architecture of epicEFI, an in-development engine management system. Mitch explains how decoders are interrupt-driven per tooth event and how timing for all outputs is computed relative to a predicted TDC epoch, using a software triggering approach rather than hardware timers. The discussion covers the main loop architecture, dual-core design, and how TDC detection feeds into ignition and injection scheduling.

## Details

### Main Loop and Software Triggering

Rather than hardware timers, epicEFI uses a software triggering system. Each Injector and Ignition instance has a trigger range consisting of two epochs in microseconds:

```c
int trigger_range[] = {start, stop};
```

The main loop on one CPU core continuously checks if the current epoch falls within any output's trigger range, and fires the output if so. Serial IO is also handled in this loop.

- Main loop duration: **ones of microseconds, usually sub-5us**
- Because the loop is so fast, outputs can be changed on the fly mid-crank-cycle
- Wasted fuel/spark is implemented by copying one trigger range to another before recalculating timing:
  ```c
  memcpy(trigger_range_a, trigger_range_b, ...);
  ```

The rest of the firmware (table indexing, sensor reads, etc.) runs on the **second CPU core**, keeping the timing loop dedicated and low-latency.

### Decoder Interrupt Model

Decoders are called each time a tooth signal creates an interrupt from the crankshaft or camshaft sensor.

The decoder's job is to decide whether the engine has reached TDC at, or just before, the current tooth. If it has, it calls `TDCEvent()`.

```
tooth interrupt fires
    -> decoder called
    -> decoder determines if TDC occurred at or before this tooth
    -> if yes: calculate TDC epoch (accounting for pre-tooth offset if TDC was between teeth)
    -> call TDCEvent(tdc_epoch)
```

For a single-tooth lawnmower-style decoder, this detection is straightforward. For multi-tooth decoders like the G4ED, **teeth counting** is used instead of a pure timing check — this is particularly important during cranking where tooth-to-tooth intervals are irregular.

### TDCEvent() Function

`TDCEvent()` takes the calculated TDC epoch as input and:

1. Takes into account the duration of a crank cycle
2. Accounts for engine acceleration/deceleration
3. Accounts for variance between the previously predicted TDC and the actual TDC
4. Calculates ignition and injection trigger ranges for all outputs around the predicted time of the **next** TDC (360 degrees ahead)
5. Handles whether outputs are wasted spark/fuel and whether cycle offsets are greater or less than 360 degrees

**TL;DR:** Decoders call `TDCEvent()` which calculates timing for ignition/injection around a predicted time of the next TDC (360 degrees ahead).

### TDC Variance Correction (Mitch's approach)

To handle engine acceleration and deceleration between TDC events, Mitch implemented a variance-correction mechanism:

1. Calculate the variance between the **predicted** TDC epoch and the **actual** TDC epoch
2. Add that variance to the next prediction

This accounts for RPM changes between cycles. It adjusts for **average acceleration over one cycle**, but does not adjust timing mid-cycle based on inter-tooth acceleration.

### Ignition Table Encoding

The ignition table is specified in **degrees**. When scheduling a spark event:

- Take the epoch of the TDC event as reference
- Convert the desired degree value (e.g., 15 BTDC) to a time epoch
- Schedule the output trigger range based on that epoch

```
spark_epoch = tdc_epoch - degrees_to_microseconds(ignition_advance)
```

### Multi-Cylinder Handling

Multi-cylinder engines are treated as a collection of independent single-cylinder engines, each with their own offset from a reference TDC. Each cylinder has a normal offset applied to the shared TDC epoch.

### Timing Precision Requirements

ggurov emphasized that timing must be accurate to approximately **0.1 degree** even while revving wildly. The discussion acknowledged that scheduling all events at a single TDC event without inter-tooth updates is insufficient for engines with significant acceleration/deceleration — the error window becomes especially tight when firing points are close to the TDC detection angle (e.g., needing to fire at 19 BTDC when TDC is detected at 20 BTDC leaves only 1 degree of window for error).

An additional edge case: if the TDC detection angle is 15 BTDC but firing is required at 20 BTDC, a full engine revolution must elapse before the next opportunity, which makes single-event scheduling problematic.
