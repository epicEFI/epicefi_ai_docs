# epicEFI Generic Trigger Wheel Descriptor: 900-Byte Bit-Encoded Format

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ggurov, @Mitch (mitch0s)*

## Summary

ggurov proposes a novel generic trigger wheel description format where the complete signal pattern of any trigger wheel is encoded as 900 bytes of bit-packed data, with each bit representing 0.05 degrees of crankshaft rotation (HIGH or LOW signal state). From this byte array, the ECU can automatically derive a tooth map, identify gap ratios for synchronization, and generalize decoder behavior to any wheel pattern. Mitch and ggurov explore the implementation details including initial sync, edge detection, and a vision for auto-generating decoders by cranking the engine and capturing the signal pattern.

## Details

### Format Specification

- **Total size:** 900 bytes = 7200 bits
- **Angular resolution:** 7200 bits / 360 degrees = **0.05 degrees per bit**
- **Encoding:** Each bit represents the expected signal state (1 = HIGH, 0 = LOW) at that angular position

The complete 360-degree trigger wheel pattern is described at 0.05-degree resolution, which is sufficient to represent any known trigger wheel geometry including those with very narrow or asymmetric teeth.

**Scalable version:** The degree step can be generalized:
```
deg_step = 360 / (array_size_bytes * 8)
```
Providing a larger array increases angular resolution proportionally.

### Edge-Only Representation

ggurov noted that there is no guarantee that the HIGH pulse width equals the LOW pulse width in a real trigger sensor — tooth geometry and sensor characteristics mean the high-time and low-time may differ. However, edges (rising and falling transitions) are expected to be at consistent angular positions.

Therefore, an edge-only encoding may be preferable over raw high/low state: rather than storing signal level at each bit position, only store the positions of rising and falling edges. This eliminates dependence on absolute duty cycle.

### Deriving a Tooth Map from the Byte Array

From the 900-byte descriptor, a tooth event map is generated programmatically:

```c
int prev_bit = 0;
uint8_t dat[900];
float deg = 0.0;
int i, j;
int bit;

for (i = 0; i < 900; i++) {
    for (j = 0; j < 8; j++) {
        bit = (dat[i] >> j) & 1;
        if (bit != prev_bit) {
            create_event(deg);   // record edge at this angle
        }
        deg += 0.05;
        prev_bit = bit;
    }
}
```

This produces a list of tooth edge angles across the full 360 degrees.

### Initial Synchronization Using Gap Ratios

During initial cranking, the ECU starts out of sync. To locate its position within the decoder pattern, the following approach is used:

1. Measure the time between consecutive tooth interrupts
2. Compare each interval to the previous interval to compute a ratio
3. A **missing tooth** (gap) produces a ratio significantly higher than 1.0 (approximately 2x or 3x for a 60-2 wheel)
4. Normal teeth produce ratios close to 1.0 (the minimum tooth ratio)

```
ggurov: "you crank, watch for time, compare to previous time, calculate ratio"
ggurov: "missing tooth has higher ratio"
```

For wheels with multiple gaps of different sizes, the number of normal-ratio teeth between gaps allows the ECU to distinguish which gap it has just seen:

- After a high-ratio gap, count the number of regular-ratio teeth that follow
- If a sufficient count (e.g., 9–10 teeth) is accumulated before the next gap, the ECU knows it is on the longer arc between the two gaps
- Fewer normal teeth between gaps indicates the shorter arc

The tooth counts between gaps can be computed automatically during system initialization by pre-processing the 900-byte array to find events between gaps.

### Auto-Decoder Vision

ggurov and Mitch discussed the potential for a "create decoder" feature in the tuning software:

1. User clicks "create decoder"
2. ECU captures the raw crank signal during a few cranking cycles
3. ECU detects repeating patterns in the signal
4. ECU generates the 900-byte descriptor automatically
5. Decoder is immediately usable without manual configuration

Mitch: "Genuinely could be the next big thing in engine-management tbh."

ggurov confirmed that rusEFI already uses a conceptually similar approach in some decoders ("rusefi decoder kinda sorta maybe could support this, it has 'create events'"), lending credibility to the design.

### Mapping Tooth Events to the Buffer (Efficient Lookup)

Mitch asked about the most efficient way to initially map tooth trigger events (from interrupts) to positions in the 900-byte buffer. The proposed approach:
- Pre-process the 900-byte array at initialization to build a compact lookup table
- Each entry in the lookup table stores the buffer index (and bit offset) corresponding to a tooth edge angle
- At runtime, the interrupt handler indexes into this lookup table rather than scanning the full buffer

This converts an O(n) scan into an O(1) lookup per tooth event.

### Relevance to rusEFI

ggurov noted that rusEFI's existing decoder infrastructure uses a similar event-table approach in some decoders, where each physical tooth maps to an event with a defined angle. The 900-byte format extends this concept to arbitrary angular resolution without requiring decoder-specific code.
