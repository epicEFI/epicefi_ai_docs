# Spark and Fuel Skip Logic, and Waste Spark V8 Configuration

*Source: rusEFI Discord, 2025-11-22 | Channel: 1401895238481744052*
*Contributors: ggurov (@ggurov), Joesphan (@joesphan), MykkH (@mykkh)*

## Summary

Explanation of the accumulator-based ignition skip algorithm used in rusEFI/epicEFI for fractional fire rate control, plus clarification of waste spark cylinder count configuration for a V8 engine.

## Details

**Ignition skip algorithm (accumulator logic):**
rusEFI/epicEFI uses an accumulator-based approach to implement fractional ignition (and injection) firing rates. The algorithm works as follows:

```
On each potential fire event:
  accumulator += adder
  if accumulator > max:
      skip this ignition event
      accumulator -= max
```

- **adder**: the fractional rate value (set by the user/tune).
- **max**: the threshold above which a skip occurs.
- This produces a mathematically even distribution of skipped events across engine cycles rather than skipping N consecutive events in a row.

A user asked whether the system can skip 1 spark then 1 fuel injection independently. The accumulator logic can be applied separately to spark and fuel channels, allowing independent skip rates for each.

**Waste spark V8 — cylinder count configuration:**
For a waste spark setup on a V8 engine, the correct configuration is **4/1** (four coils, one spark per coil per cycle, firing two cylinders simultaneously — one on compression, one on exhaust stroke). MykkH confirmed this is the expected setting for an 8-cylinder waste spark arrangement.

**100 MHz timer and RPM resolution:**
MykkH asked whether the 100 MHz hardware timer begins losing resolution at high RPM. The answer is that theoretical resolution limits exist but are not practically relevant for normal automotive engines:
- At very high RPM (above ~10,000 RPM), the timer tick intervals between tooth edges become short enough that quantization noise could theoretically matter.
- For typical performance engines operating below 10,000 RPM, the 100 MHz timer provides more than sufficient resolution and this is not a concern.
