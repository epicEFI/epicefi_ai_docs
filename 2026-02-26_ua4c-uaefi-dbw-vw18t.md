# UA4C / uaEFI DBW Integration Options for VW 1.8T

*Source: rusEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @StarKiller, @EpicEngineer, @Ognjen Galic*

## Summary

A user planning to install rusEFI on a VW 1.8T engine asked about keeping Drive-By-Wire (DBW) throttle control while using a UA4C ECU. The UA4C does not have native DBW support, so the recommended path is to use a uaEFI board instead, which supports the ME7.5 throttle body. For users on a budget, two options were outlined: uaEFI with ME7.5 throttle hardware, or abandoning DBW entirely and running a pure mechanical cable setup.

## Details

### Problem: UA4C Lacks Native DBW Support

- The UA4C ECU does not provide direct support for Drive-By-Wire (DBW/ETB) throttle control.
- A user noted that a uaEFI board listed on the rusEFI marketplace supports DBW and is the preferred alternative.

### Recommended Options (Budget-Oriented)

1. **uaEFI + ME7.5 throttle body + diesel pedal + wiring** — This combination gives full DBW capability using widely available VAG hardware.
2. **Pure cable throttle** — Drop DBW entirely and use a mechanical cable linkage; simpler but loses electronic throttle control.

### H-Bridge Requirement

- An H-bridge is needed specifically for idle control (idle air motor / idle stepper).
- The H-bridge is **not** required to run the engine; it is optional for idle quality.
- The idle portion can alternatively be implemented using full DBW (using the ETB routine to handle idle setpoint).

### STM32 Adapter vs. Direct rusEFI

- Questioned whether an STM32 adapter for the UA4C or a direct rusEFI board is preferable.
- Community consensus leaned toward going directly with a uaEFI/rusEFI board (such as uaEFI) rather than adapting the UA4C via STM32.

### Diesel Pedal Sensor Note

- VAG diesel throttle pedal sensors may have only **one** analog output, unlike petrol equivalents which have two.
- This is relevant when considering DBW pedal demand sensing — verify the pedal type before assuming dual-pot input.
