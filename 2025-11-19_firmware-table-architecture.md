# Firmware Table Architecture and Chained Table Outputs

*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @mitch0s*

## Summary

@mitch0s shared his approach to organizing lookup tables in ECU firmware for scalability, settling on keeping table instances decoupled from their consumers. He also described a future goal of allowing table outputs to chain as inputs into other tables, enabling complex interdependent control strategies.

## Details

### Table Instance Separation from Consumers

- After trying multiple architectures, @mitch0s concluded that table instances should be kept **separate from the objects that consume them**
- Consumer classes that reference tables include: `FuelScheduler`, `IgnitionScheduler`, `AnalogueSensor`, `DigitalSensor`
- Keeping tables as standalone instances (rather than embedding them inside consumer classes) improves scalability: the same table instance can be referenced by multiple consumers, and tables can be swapped or modified independently of the consuming logic

### Chained Table Outputs as Inputs

- Goal: allow the output value of one lookup table to be used as an input axis for another lookup table
- This would enable nested correction strategies, for example:
  - A VE table corrects for load, and its output feeds into a charge temperature correction table
  - A table could theoretically reference its own output for iterative/feedback-style calculations
- This is described as a future/aspirational feature — not yet implemented at the time of this discussion

### Context

- This discussion occurred alongside a broader conversation about firmware architecture choices for a custom ECU being developed from scratch by @mitch0s
- The same session included discussion of 2D/3D table instances handled by the same class with different read modes
- @mitch0s was also considering Qt+Python for the tuning software side, compiled to native executable via Nuitka
