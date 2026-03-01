# Spark Cut / Rotational Idle — Accumulator-Adder Algorithm

*Source: rusEFI Discord, 2025-11-21 | Channel: general-tuning (1401895238481744052)*

*Contributors: ggurov, SanGawku, MykkH*

## Summary

ggurov implemented a first-pass spark cut algorithm for epicEFI's "rotational idle" (cylinder-selective misfire for sound/exhaust crackle effects). The algorithm uses an **accumulator + adder + max** pattern rather than a simple "cut every Nth spark" to allow more varied cut patterns. The initial implementation is a forced misfire with no ETB/idle compensation yet. MykkH observed the logic analyzer showing the cut firing in reverse firing order on a 2JZ and identified that the accumulator step size determines which cylinders get cut.

## Details

### Algorithm Description

- For every spark event, add a configurable **adder** value to an **accumulator**.
- When the accumulator reaches the configured **max** value, **skip (cut) that spark** and reset or wrap the accumulator.
- This produces a repeating cut pattern whose density is controlled by the adder/max ratio.
- ggurov: *"for every spark, add $adder to accumulator up to max, when max is reached, skip spark"*

### Behavior with Different Adder/Max Values

- ggurov noted that **prime number** adder values produce the most interesting / least-repeating patterns, though non-prime values also work and create different sounds.
- SanGawku suggested a simpler "cut X from Y events" framing; ggurov acknowledged this is mathematically equivalent from the other side of the ratio.
- ggurov: *"I think it needs more thought and some multipliers, so it gets to cutting all channels, every X fires"* — the algorithm is designed to scale from light misfire to full cylinder cut.

### Firing Order Observation (2JZ)

- MykkH ran the logic analyzer on a 2JZ (firing order 1-5-3-6-2-4).
- Observed that with certain accumulator settings, ignition events were being skipped in **reverse firing order** (1-4-2-6-3-5), because the accumulator was stepping every 5th cycle rather than every 7th.
- This confirms the algorithm operates across all cylinders in sequence and the cut cylinder depends on which spark event hits the accumulator limit.
- MykkH: *"it looks like skipping ignition cycle in reverse firing order... probably because it's skipping every 5th cycle instead of 7th?"*

### Current State (as of 2025-11-21)

- Initial implementation: **forced misfire only** — the targeted cylinder's spark is suppressed.
- **No ETB correction or idle air compensation** implemented yet (ETB/idle add was noted as a TODO).
- No fuel cut accompanies the spark cut in this initial version.

### Per-Cylinder Injector Pulsewidth Logging

- Related request from MykkH: *"would it be a royal pain to add injector pulsewidth by cylinder?"*
- At the time, the 'Fuel: last injector pulsewidth' channel in datalogs only updates with cylinder 1 each cycle.
- ggurov acknowledged this as a possible addition: *"pw by cyl maybe"*
- Per-cylinder logging already exists for: ignition timing, MAP, spark event count, injection event count.
