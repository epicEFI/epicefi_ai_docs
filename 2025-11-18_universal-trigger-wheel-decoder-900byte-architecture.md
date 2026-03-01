# Universal Trigger Wheel Decoder Architecture: 900-Byte Bitmap Approach

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ggurov, @mitch0s, @yeoldepirate, @lysdex*

## Summary

A detailed design discussion on a universal trigger wheel decoder architecture for custom ECU firmware. The core idea, proposed by @ggurov, is to represent any trigger wheel pattern as a compact byte array (nominally 900 bytes for 0.05-degree resolution per bit over 360 degrees), from which edges are extracted at known crank angles. This approach would allow any trigger wheel pattern to be described uniformly, enabling a single universal decoder function instead of individual hard-coded decoders per engine. The discussion also covers initial engine synchronization strategies, missing-tooth detection, and the limitations of the TDC-to-TDC averaging method for timing stability.

## Details

### The 900-Byte Trigger Wheel Representation

@ggurov proposed representing each trigger wheel decoder as 900 bytes of data, where each bit encodes whether the sensor signal is HIGH or LOW at a given 0.05-degree increment of crankshaft rotation:

- 900 bytes x 8 bits = 7,200 bits
- Each bit = 360 / 7200 = 0.05 degrees of crank angle
- Total coverage = 360 degrees (one crank revolution)

The degree increment per bit can be calculated generically as:

```
degree_increment = 360 / (ary_size * 8)
```

where `ary_size` is the number of bytes in the array. This allows variable-resolution descriptions by simply changing the array size.

Rather than encoding raw high/low levels, it may be preferable to encode only edges (rising/falling transitions), since there is no guarantee that sensor HIGH duration equals sensor LOW duration on a real trigger wheel. Edges, however, are expected to be the same distance apart.

### Event Creation from the Byte Array

Given the byte array, creating ECU events (trigger interrupts at specific crank angles) is straightforward:

```c
int prev_bit = 0;
uint8_t dat[900];
float deg = 0;
int i = 0;
int j = 0;
int bit;

for (i = 0; i < 900; i++) {
    for (j = 0; j < 8; j++) {
        bit = (dat[i] >> j) & 1;
        if (bit != prev_bit) {
            create_event(deg, bit);
        }
        deg += 0.05;
        prev_bit = bit;
    }
}
```

Iterate through the array, shift bits, increment the degree accumulator, and call `create_event()` whenever the bit value changes. The rusEFI decoder framework already uses a `create_event` concept; this byte-array approach generalizes it.

The ArduStim project (Speeduino's trigger wheel simulator) uses a similar concept for encoding wheel patterns:
- Reference: https://github.com/speeduino/Ardu-Stim/blob/master/ardustim/ardustim/wheel_defs.h (see around line 305)

### All Decoders Feed a Common Timing Function

The broader architecture proposed is that all decoders—regardless of trigger wheel type—calculate the crankshaft position in degrees for each tooth and pass that value to a central `updateTiming()` function. This means:

- More teeth on the wheel = more frequent position updates = more stable timing
- The universal decoder provides a uniform data source regardless of engine variant
- Decoders essentially "turn different triggers into single-tooth wheel signals" for the timing engine

CPU time (ticks) is decoupled from physical crankshaft position; timing only stays accurate when the engine runs at approximately the average speed assumed by the algorithm.

### TDC-to-TDC Averaging Method

@mitch0s was using a TDC-to-TDC averaging approach for his custom ECU (tested on a lawnmower engine). This method:

- Adjusts for average engine acceleration/deceleration once per cycle
- Does NOT compensate for within-cycle acceleration (mid-cycle variance)
- Is simpler to implement and acceptable for steady-state operation

A concern was raised about whether an engine could accelerate fast enough over 360 degrees to cause >=1 degree of timing variance. Consensus was yes, in principle (confirmed via external sources), though it primarily matters under aggressive acceleration or high-RPM conditions rather than steady-state idle.

The TDC-to-TDC method was confirmed to work in practice with the specific lawnmower engine tested: the crankshaft trigger pattern had two magnets — one reading at 60° BTDC and one directly at TDC.

### Initial Engine Synchronization (Sync Detection)

The key challenge is how the ECU initially synchronizes to the trigger wheel position during cranking. The approach depends on the wheel type:

**Single missing tooth (e.g., 36-1, 60-2):**
- A single missing tooth creates a gap whose period ratio is higher than the normal tooth period (approximately `(N+1)/N` times the normal gap)
- Detect the large gap ratio, then count a defined number of normal-ratio teeth before or after the gap to confirm position
- "Normal" tooth ratio = minimum tooth ratio (shortest measured inter-tooth interval)
- Algorithm: count teeth past the large-gap event; once count Z is reached, the ECU knows it started counting at a specific known crank angle. This is simpler and less processing-intensive than sliding-window comparison approaches.

**Programmatic tooth-gap detection:**
- On startup (init), pre-process the 900-byte array to calculate the number of normal teeth between large gaps
- The minimum tooth ratio can be found by calculating the 3-4 smallest gaps observed and taking the smallest as the baseline

**Determining regular gap size during cranking:**
- Bucket-count gap ratios during cranking (e.g., group within 75% of each other)
- The most common bucket is the normal tooth gap; outliers are missing teeth or large gaps

**Condition for reliable sync:**
- The algorithm works as long as there are more normal teeth than gaps in the pattern
- @mitch0s noted this constraint but observed it would not actually matter in most cases since teeth-to-gap ratios are always high

**Symmetrical wheels requiring cam signal:**
- Wheels with two equal missing-tooth sections 180 degrees apart (e.g., Nissan QR25DE) cannot be decoded with crank signal alone
- These require a cam signal to distinguish which half of the 720-degree cycle the engine is in
- For such engines, the decoder would need a 900*2-bit stream (or equivalent) combining crank + cam
- Waste spark ignition cannot be used on these engines

### Trigger Wheel GUI and TDC Adjustment

@mitch0s proposed a trigger wheel editor in the tuning software GUI:

- A table-based editor with rows like:
  ```
  section 1 : raised : 1deg
  section 2 : flat   : 2deg
  section 3 : raised : 1deg
  ...
  ```
  with a graphical display of the resulting trigger wheel shape
- An "adjust TDC" dial that spins the 900-byte bitmap from 0-360 degrees, effectively adjusting the starting point in the byte array — allowing TDC offset calibration without regenerating the entire decoder

### Auto-Detect Wheel Feature (Future Concept)

@ggurov proposed an "autodetect wheel" feature in the tuning software:

- The user cranks the engine over several times
- The ECU captures crank signal edges and detects patterns (number of teeth, gap locations, gap ratios)
- The software generates a visual drawing of the inferred wheel pattern
- The user then confirms or specifies known crank angle reference points
- Standard patterns like 36-2 or 60-2 (one tooth count + one large gap) could be autodetected in 3-4 revolutions

Alternatively, a logic analyzer + Python script could be used offline to generate the initial 900-byte bitmap from captured signals.

### Decoder Portability Benefits

With a universal decoder function, trigger wheel definitions become separate data files rather than firmware code. Benefits:

- Users could download decoder definitions for their engine rather than needing a new firmware version
- The question "is my engine supported?" would almost always be "yes" for the generic decoder
- Exception: very high-resolution wheels (e.g., early 1990s Nissan with 360 slits / 720 interrupts per revolution) may require hardware-level handling

### Injector Duration Table and Interpolation

@mitch0s observed RPM drops when removing fuel from specific cells in the injector duration (pulse width) table. The issue was identified as non-interpolated table reading — the ECU was snapping to the nearest cell value rather than interpolating between cells, causing step changes in fueling and corresponding RPM instability.

### Sequential Fuel Injection vs. Batch

For fuel-only ECU applications, a distributor trigger signal may be sufficient. However, running sequential injection (rather than batch) has advantages:

- Better acceleration enrichment response, as fuel is injected into a moving air stream rather than sitting in the manifold
- Requires both cam and crank signals for proper cylinder identification
- Batch fire ("YOLO") also works and is simpler to implement
