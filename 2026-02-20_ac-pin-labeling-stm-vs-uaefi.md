# AC Pin Labeling Discrepancy: STM Names vs uaeFI Names in Firmware

*Source: rusEFI Discord, 2026-02-20 | Channel: 1401895238481744052*
*Contributors: @DCwerx*

## Summary

In the 2026-01-06 firmware, the AC request and AC relay pins are labeled using STM microcontroller pin naming conventions rather than the board-level uaeFI pin names. This discrepancy was observed by a community member while reviewing the firmware configuration and may cause confusion when mapping pins in TunerStudio or similar tooling.

## Details

### Observation

User @DCwerx reported that when examining the 2026-01-06 firmware:

- The **AC request** pin and the **AC relay** pin are labeled with **STM pin names** (e.g., `PA0`, `PB3` style identifiers) rather than the human-readable **uaeFI board pin names** used elsewhere in the configuration.
- This naming inconsistency only appears to affect the AC-related pins; other pins use the expected uaeFI naming scheme.

### Impact

- Users configuring AC control (compressor clutch request/relay) may need to cross-reference the STM pin name against the board's pinout diagram to identify the correct physical connector pin.
- No firmware-side fix was confirmed in this thread; the status of the fix in later firmware versions is not documented here.

### Affected Firmware Version

- **2026-01-06** firmware confirmed to have this labeling inconsistency.
