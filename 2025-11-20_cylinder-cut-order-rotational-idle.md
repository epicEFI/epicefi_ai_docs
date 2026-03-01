# Cylinder Cut Order for Rotational Idle / Sequential Cylinder Kill

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov, ZeeKay, MykkH*

## Summary

When implementing a rotational (rolling) cylinder cut — where cylinders are killed in sequence to produce a burbling idle effect or to reduce power progressively — the cylinders should be skipped in firing order, not in cylinder number order. The pattern skips every `(cylinderCount + 1)`th firing event within the firing order sequence.

## Details

### Cut Pattern (4-cylinder example, firing order 1-3-4-2)

For a 4-cylinder engine with firing order 1-3-4-2, the rotational cut sequence is:

```
1 (SKIP), 3, 4, 2, 1, 3 (SKIP), 4, 2, 1, 3, 4 (SKIP), 2, ...
```

Each cylinder in the firing sequence gets skipped every `cylinderCount + 1` firings (i.e., every 5th event for a 4-cylinder). This ensures each cylinder gets approximately equal treatment and the power reduction is smooth and even.

### Implementation Notes

- The Ignition Hardware tab in TunerStudio has per-cylinder kill buttons that can be toggled manually for testing.
- To test the rotational cut pattern without scripting, you can use the existing **Dynamic Launch Control** feature, which already uses cylinder cuts. Enabling launch control temporarily while stationary allows verification that the cut pattern and RPM behavior are working correctly.
- ggurov noted: "you can test it now with dynamic launch control, cause that will cut cyl."
- MykkH noted: "Technically it's in firing order" — confirming the skip should follow the engine's firing sequence, not a fixed numerical cylinder order.

### Formula

For an N-cylinder engine with firing order `[c1, c2, ..., cN]`:

- Skip the cylinder at position `(i mod N)` in the firing order every `(N + 1)` fire events.
- Cycle restarts after all cylinders have been skipped once across `N * (N + 1)` total fire events.
