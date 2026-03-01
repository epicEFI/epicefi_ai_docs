# Injector Pulse Width Resolution vs VE Table Granularity

*Source: rusEFI Discord, 2026-02-20 | Channel: 1401895238481744052*
*Contributors: @MykkH*

## Summary

Bench testing revealed that injector pulse width steps in increments of 0.0033 ms, and that approximately 0.16-0.2 VE table units are required to advance the pulse width to the next available step. This has implications for how finely the VE table can actually affect fueling in practice.

## Details

### Measured Behavior

During bench testing with a specific hardware configuration:

- **Injector PW step size**: 0.0033 milliseconds per step
- **VE units required per PW step**: ~0.16 VE (meaning it takes roughly **0.2 VE change** for the injector pulse width to increment to the next discrete value)

### Implications for VE Table Resolution

This means that small VE table adjustments (less than ~0.16-0.2 units) have **no effect** on actual injector output — the hardware cannot resolve changes smaller than one PW step. In practice:

- The commonly used XX.X (one decimal place) VE table format may have more resolution than the hardware can actually deliver at fine-grained adjustments.
- The even finer XX.XX (two decimal place) format would provide even less meaningful differentiation per step.

### Community Discussion Context

This was raised in the context of a discussion about whether allowing tenths or hundredths decimal place selection in the VE table would be a useful user-facing feature. The conclusion was that while the finer resolution does not change the actual injector behavior step size, it could still be useful for user clarity or future hardware with higher resolution.

> "I still think allowing the tenths or hundredths decimal selectable is a neat feature for the user." — @MykkH
