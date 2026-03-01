# TunerStudio Array Indexing and updateTunerStudioState() Trigger

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat (1356732771325968630)*
*Contributors: @ggurov*

## Summary

Two brief but technically useful notes from ggurov: TunerStudio transmits array indices (not values) when referencing array items, and the firmware function `updateTunerStudioState()` runs on every relevant event cycle.

## Details

### TunerStudio Sends Array Index, Not Value

When TunerStudio communicates with the ECU about an array-valued parameter, it sends the **index** of the selected array element rather than the value at that index:

> "tunerstudio sends the index of the array item" — @ggurov

This is important when implementing firmware-side handling of array-type configuration parameters. The firmware must use the received index to look up the actual value from the array stored in its own memory, rather than expecting the raw value to arrive over the serial protocol.

### updateTunerStudioState() Trigger

The firmware function `updateTunerStudioState()` executes on every relevant event:

> "it runs every `void updateTunerStudioState() {`" — @ggurov

This function is responsible for refreshing the data snapshot that TunerStudio reads (gauge values, table data, sensor readings, etc.). Understanding when this function is called is relevant when adding new output variables or debugging why a value is not updating in the TunerStudio UI — if a variable is not written inside `updateTunerStudioState()`, it will not be visible or updated in the tuning software.

### Square Wave Output

In the same discussion thread, Joesphan asked whether it is possible to output a square wave via epicEFI:

> "Can I output a square wave for ex" — @Joesphan

This indicates a user interest in generating a configurable frequency square wave on a digital output pin — a feature relevant for driving external devices (fuel pump priming pulses, test signals, etc.). The specific answer from the firmware side was not captured in this thread.
