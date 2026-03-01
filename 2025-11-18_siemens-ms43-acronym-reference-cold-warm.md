# Siemens MS43 ECU: Acronym Reference and Cold/Warm Transition Logic

*Source: rusEFI Discord, 2025-11-18 | Channel: 1389299516808630312 (theory/dev)*
*Contributors: @ggurov, @joesphan*

## Summary

@ggurov shared two reference resources from the Siemens MS43 ECU world: a link to the ms4x.net wiki page covering the cold-to-warm engine operation transition logic, and a partial listing of the Siemens keyword/abbreviation translation table used in MS43 map names. The keyword table explains the cryptic abbreviations embedded in Siemens ECU variable and map names.

## Details

### Cold/Warm Engine Operation Transition

The Siemens MS43 ECU (used in BMW E46 and others) has documented logic for transitioning between cold and warm engine operation modes. This affects fueling, idle, and various compensation strategies.

Reference: https://www.ms4x.net/index.php?title=Siemens_MS43#Transition_between_cold/warm_engine_operation

### Siemens Keyword/Abbreviation Translation

Siemens ECU map and variable names use a structured abbreviation system. The full list is available at:
https://www.ms4x.net/index.php?title=Siemens_Keyword_Translation

A sample of the "D" section abbreviations (as shared by @ggurov):

| Abbreviation | Meaning |
|---|---|
| DHP | dashpot |
| DI | disable |
| DIAG | diagnosis |
| DIAGCP | diagnosis canister purge |
| DIAGCPS | diagnosis canister purge solenoid |
| DIF | difference |
| DIG | digital |
| DIGIS | digital idle speed stabilization with ignition |
| DIP | differential pressure |
| DIR | direction |
| DIRE | directory |
| DIS | discrete |
| DISP | display |
| DIST | distance |
| DIV | division |

A sample of the "A" section abbreviations (partial, also shared by @ggurov):

| Abbreviation | Meaning |
|---|---|
| ACQ | acquisition |
| ACR | actuator |
| ACT | active |
| AD | adaptive |
| ADC | advance (early) |
| ADD | additive |
| ADJ | adjustment |
| ADR | address |
| AE | acceleration enrichment |
| AEB | active engine brackets |
| AETCU | autarchic electronic transmission control unit |
| AFL | air fuel lean |
| AFR | air fuel rich |

### Example of a Full Siemens Map Name

@ggurov provided this example of a Siemens ECU variable name and how it is parsed:

```
ip_cam_sp_tco_1_in_is__n__maf_iv
```

This name is built by combining short keyword fragments — each fragment maps to a word or phrase via the lookup table. The lookup system itself has a recursive lookup: there is a table of name partials, and those partials are looked up to construct the full meaning.

### Commentary

@ggurov's reaction to first encountering the full depth of Siemens naming: "it goes a level deeper — there's a lookup for the table name partials." The naming convention is described as cryptic enough that @fast335xi noted having to "translate thousands of maps, thank god for WinOLS" when working with BMW firmware.
