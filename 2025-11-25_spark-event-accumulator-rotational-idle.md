# Spark Event Accumulator Logic and Rotational Idle Pattern

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Jmoneus (@jmoneus)*

## Summary

Explanation of the spark event accumulator mechanism used in rusEFI/epicEFI to implement the "rotational idle" feature — a per-cylinder timing pattern that produces an uneven idle sound by selectively skipping or delaying spark events based on an accumulator with a configurable adder and maximum threshold. Also covers the firmware customization entry point (table size changes) for users new to custom firmware.

## Details

**Spark event accumulator logic:**
The rotational idle feature works by maintaining a running accumulator value. At each spark event opportunity, a configurable "adder" value is added to the accumulator. If the accumulator exceeds the maximum threshold, that spark event is skipped (suppressed) and the accumulator is decremented by the maximum value (wraps around). This produces a rhythmic pattern of fired and skipped cylinders, creating an uneven idle "lope" sound.

Algorithm in pseudocode:
```
accumulator += adder
if accumulator > max:
    skip this spark event
    accumulator -= max
```

The pattern is controlled by three parameters per layer: the adder value, the max value, and there are three pattern layers that combine to create the final output. All sound variation in the rotational idle video demonstration was produced by just two numbers (adder + max) per active layer.

> "Every spark event adder is added to accumulator. If accumulator is over max, skip that spark event — max to accumulator." — ggurov
> "There are 3 layers for the pattern."
> "All of those sounds in that video are just 2 numbers." — ggurov

**Firmware customization entry point — table size:**
For users interested in modifying rusEFI/epicEFI firmware, increasing lookup table resolution is described as one of the easiest starting changes. Tables can be expanded from the default size (commonly 16×16 on Speeduino) to 32×32 or 64×64. This change is handled in configuration rather than deep firmware logic.

Benefit of larger tables: eliminates "bin anxiety" — the concern about allocating enough cells in idle/low-load regions while still retaining sufficient resolution at high load/rpm. With 32×32, there are 4× as many cells as 16×16, so each operating region can be mapped independently without sacrifice.

> "Easiest is changing size of tables — 32×32, 64×64."
> "It gets rid of bin anxiety. 'I can't waste bins on this idle here, because I won't have enough cells up top to do the rest of the tune.' That no longer exists with 32×32." — ggurov

**Fuel table self-tuning:**
Once the engine is running with correct sensor inputs, the fuel table can be auto-tuned via closed-loop feedback from the wideband O2 sensor. Cruise/steady-state load points can be dialled in within 10–15 minutes of stable running. Acceleration enrichment requires more time and is not auto-tuned in the same way.

> "Fuel tunes itself... as long as you have proper inputs, fuel cruise state you can tune in 10–15 minutes after running reliably." — ggurov

**Tuning approach — street vs dyno:**
For rusEFI/epicEFI setups, street tuning is generally recommended over dyno tuning, primarily because specialised dyno operators are unlikely to be familiar with the software. Power/WOT cells can be tuned from data logs. Drivability tuning (part-throttle tip-in, idle stability) requires more iterative real-world driving regardless of dyno access.
