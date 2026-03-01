# epicEFI Inter-Tooth Timing Updates and Real-Time Epoch Correction

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @Mitch (mitch0s), @ggurov*

## Summary

ggurov and Mitch discuss the necessity of recalculating all scheduled output epochs on every tooth interrupt, not just at TDC. This is required because engine speed is not constant across a crank cycle — acceleration and deceleration between teeth make TDC-only scheduling inaccurate. The conversation proposes an `updateTiming()` function that takes the current engine position in degrees and adjusts all scheduled epochs, analogous to how `TDCEvent()` works but triggered at every tooth.

## Details

### The Problem with TDC-Only Scheduling

Scheduling all output events based solely on the TDC epoch and then not updating them until the next TDC event causes errors under acceleration or deceleration. The key insight is:

> "CPU time (cpu ticks) is totally disconnected from the physical thing we're tracking, and just so happens to line up if it's going the same average speed."
> — ggurov

Multi-tooth trigger wheels exist specifically to provide frequent position updates. Each tooth crossing is an opportunity to compare actual elapsed time against predicted elapsed time and correct accumulated error.

ggurov stated: "every time you see a tooth come by, you effectively have to recalculate the scheduled epoch for all of the outputs — because it will have changed since the last time."

### TDC_EPOCH Direct Compensation

ggurov proposed directly compensating `TDC_EPOCH` on every tooth interrupt (or even every edge), not just at TDC detection:

1. On each tooth interrupt, determine the current tooth number
2. Adjust `TDC_EPOCH` based on the tooth's expected angular position
3. Recalculate `tdc_prediction` from that adjusted epoch
4. Recompute all scheduled output epochs from the updated prediction

This approach is more aggressive than variance-only correction and accounts for within-cycle speed changes.

### Proposed updateTiming() Function

Mitch proposed a new function analogous to `TDCEvent()`:

```
updateTiming(engine_position_degrees)
```

- Called by each decoder every time a tooth interrupt fires
- Takes current engine position in degrees (calculated by the specific decoder)
- Adjusts all scheduled output epochs based on measured acceleration/deceleration between teeth
- Uses a **moving average** to smooth tooth-to-tooth timing variations
- Would be implemented universally — all decoders must call it, supplying the current angular position

This separates concerns cleanly:
- **Decoder responsibility:** compute current angle from tooth events and call `updateTiming(degrees)`
- **Core responsibility:** translate angle + timing advance table into actual microsecond epochs for outputs

### Timing Stability vs. Tooth Count

The more teeth a trigger wheel has, the more frequently `updateTiming()` can be called, and the more stable the timing becomes. The relationship is direct:

- Fewer teeth → larger angular gaps between updates → more accumulated error under acceleration
- More teeth → smaller angular gaps → finer-grained correction, better timing accuracy

Example from the discussion:
- 60 teeth (60-2 wheel) → update every 6 degrees
- 24 teeth → update every 15 degrees

ggurov noted the effect is visible as compression waves in tooth-time plots captured at cranking speed — tooth timing fluctuates noticeably around TDC due to compression stroke loading, and this is present at running RPMs as well (though less visible).

### Speeduino Comparison

Mitch asked how Speeduino handles timer rescheduling when a new epoch is calculated. ggurov was uncertain of the implementation details ("that part is deep black magic"), but the question was whether Speeduino cancels and restarts hardware timers per tooth event.

The epicEFI software-trigger approach avoids this issue by not using hardware timers — the main loop simply evaluates trigger ranges continuously, so updating a range takes effect on the next loop iteration (within microseconds).

### Universality Constraints

ggurov cautioned that a truly universal inter-tooth timing function cannot handle every possible decoder due to irregular wheel designs with teeth in non-uniform positions. The practical scope is standard decoder families:
- 60-2 missing tooth
- Standard multi-tooth patterns with uniform tooth spacing

For decoders with non-uniform spacing, the degrees-per-tooth value cannot be assumed constant and the decoder must supply accurate angle values for `updateTiming()` on a per-tooth basis.
