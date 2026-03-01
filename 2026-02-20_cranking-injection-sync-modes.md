# Cranking Injection Modes: Batch/Wasted Spark Before Full Sync vs Full Sequential

*Source: rusEFI Discord, 2026-02-20 | Channel: 1401895238481744052*
*Contributors: @MykkH, @ggurov, @tmbryhn*

## Summary

This thread clarifies the distinction between available cranking injection and ignition modes in rusEFI/epicEFI, specifically the difference between waiting for full cam/crank synchronization before firing (true sequential) versus firing in wasted spark/batch mode prior to achieving full sync. A separate but related observation notes that a single "revolution of no activity" (no spark or fuel) occurs during cranking in wasted/batch mode.

## Details

### Available Cranking Injection Modes

rusEFI/epicEFI offers the following injection modes selectable during cranking:

- **Batch** — Injectors fire in grouped pairs, not synchronized to individual cylinder events.
- **Sequential** — Injectors fire timed to each individual cylinder's intake stroke; requires full cam/crank sync.
- **Simultaneous** — All injectors fire at the same time.

### Pre-Sync Behavior: Wasted Spark / Batch Until Full Sync

User @tmbryhn described the desired behavior and confirmed the firmware supports it:

> "You want the engine to fire in wasted/batch until full sync is attained."

This is distinct from the "true full sequential" behavior described by @ggurov, which withholds all fuel and spark until the ECU has achieved full cam/crank synchronization:

> "you're looking for true full sequential, no sync no fuel no spark"

The **batch/wasted spark before full sync** approach allows the engine to begin firing as soon as crank signal is valid, without waiting for cam phase, then transitions to sequential once full sync is acquired.

### RPM-Based Mode Transition

@tmbryhn noted the mode transition context: the injection mode setting applies when **RPM exceeds the cranking RPM limit**. Below that limit, the cranking-specific injection mode is active; above it, the running mode takes over.

### PRE_SYNC Fuel Pulse

During engine startup, all injector channels show an initial fuel pulse labeled `PRE_SYNC`. This is a brief fuel event that occurs before the ECU has established full synchronization with the crank/cam signals:

> "see that thing across all injector channels at the start? that's PRE_SYNC fuel" — @ggurov

This is expected behavior and visible in the logic analyzer / injector channel logging.

### "No Nada" Revolution During Cranking

@MykkH reported observing a single revolution with no spark and no fuel delivery during cranking in wasted/batch mode (top four spark outputs, bottom four fuel outputs all inactive for one cycle):

> "I too get a revolution of No Nada in waste/batch during cranking (top four spark, bottom four fuel). It must be a setting somewhere."

This may be related to the PRE_SYNC period or a timing configuration issue. No definitive resolution was provided in this thread, but it was noted as a known observation worth investigating in settings.
