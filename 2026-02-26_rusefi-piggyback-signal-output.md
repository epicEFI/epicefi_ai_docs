# rusEFI ECU Signal Output to Factory ECU (Piggyback Mode)

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @tren, @Robb235*

## Summary

A user asked whether a rusEFI ECU can output VR crankshaft and Hall effect cam signals to a factory ECU — for example, in a piggyback configuration where rusEFI processes sensor inputs and re-emits the signals to the stock ECU. The thread opened the question but did not reach a definitive answer; follow-up context about the specific vehicle and exact configuration was requested.

## Details

### The Question

User @tren asked: *"Is it possible to give signal on the factory ECU (VR crank, Hall cam) from a rusEFI ECU?"*

This describes a scenario where:
- The **rusEFI ECU** reads crank (VR) and cam (Hall effect) sensor signals
- The rusEFI ECU then **re-outputs** those signals to the **stock/factory ECU**
- This would allow rusEFI to act as a signal processor or trigger wheel emulator while the factory ECU still receives timing inputs

### Context Requested

@Robb235 asked for clarification:
- Is the rusEFI ECU being used in **piggyback mode** alongside the stock ECU?
- What **vehicle** is this being attempted on?

### Notes

This pattern (rusEFI emitting trigger signals to another ECU) is an unusual use case. The feasibility depends heavily on:
- Whether the target factory ECU accepts externally driven VR/Hall signals without the actual sensor present
- The signal levels and impedance requirements of the factory ECU inputs
- Whether rusEFI's trigger output (tooth-by-tooth emulation) can be mapped to a compatible output pin

This thread did not reach a resolution — further investigation or forum/wiki research is recommended for anyone attempting this configuration.
