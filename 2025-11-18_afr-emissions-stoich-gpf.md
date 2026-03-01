# Air-Fuel Ratio, Emissions, and Stoichiometric Targeting

*Source: rusEFI Discord, 2025-11-18 | Channel: 1392172039430738111*
*Contributors: @Joesphan*

## Summary

Brief but clear discussion on the emissions implications of rich and lean fuel mixtures, the importance of targeting stoichiometric AFR, and the introduction of gasoline particulate filters (GPF) in EU vehicles.

## Details

### Effect of Rich Mixture on Emissions

Running rich (excess fuel relative to air) produces more CO2, but catalytic converters can process the excess:

> "if you run rich you make more co2, which the cat can burn off" — @Joesphan

A properly functioning catalytic converter can oxidize the extra CO produced by a rich mixture, converting it to CO2. This means modest enrichment can be tolerated by a cat-equipped vehicle without major emissions consequences.

### Effect of Lean Mixture on Emissions

Running lean (excess air relative to fuel) drives a different emissions problem:

> "if you run lean it makes more nox" — @Joesphan

Lean combustion raises peak cylinder temperatures, which promotes the formation of nitrogen oxides (NOx). Unlike CO, NOx cannot be easily managed by a simple oxidation catalyst — it requires a three-way catalyst operating near stoichiometry, or a NOx aftertreatment system.

### The Stoichiometric Target

The practical takeaway is that running at stoichiometry (lambda = 1.0, approximately 14.7:1 AFR for gasoline) is required for a three-way catalytic converter to work efficiently. The three-way cat simultaneously reduces HC, CO, and NOx only within a narrow window around lambda = 1:

> "gotta hit stoich" — @Joesphan

For emissions compliance and catalytic converter health, the engine should target stoichiometric AFR during normal closed-loop operation.

### Gasoline Particulate Filters (GPF) in EU Vehicles

@Joesphan noted that modern EU-market vehicles now include an additional emissions device:

> "EU has gasoline particulate filters now"

GPFs are analogous to diesel particulate filters (DPF) but applied to gasoline direct injection (GDI) engines, which produce fine particulate matter from the direct injection process. This is relevant when modifying or tuning EU-spec vehicles, as the GPF adds backpressure and has regeneration requirements that must be considered.
