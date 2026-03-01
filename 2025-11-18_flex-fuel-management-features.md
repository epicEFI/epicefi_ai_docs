# Flex Fuel Management: Features and Behavior (2025-05-13 Firmware)

*Source: rusEFI Discord, 2025-11-18 | Channel: epicEFI-announcements (1376346219097620492)*
*Contributors: @ggurov*

## Summary

A significant firmware update (changelog dated 2025-05-13) introduced comprehensive flex fuel support into epicEFI. The implementation covers sensor fallback, fuel trim behavior, cranking enrichment, nitrous integration, CAN dash output, and AFR target display corrections. Stoichiometric ratios are calculated automatically based on measured ethanol content.

## Details

### Flex % Sensor with Fallback

- A `FallbackSensor` was added for flex fuel that proxies the physical flex fuel sensor.
- If no flex fuel sensor is present, the system falls back to a user-entered static override flex % value.
- Flex fueling logic is **always active**: the ECU always applies ethanol-adjusted fueling; the only question is whether the % comes from a live sensor or the static override.

### Stoichiometric Ratio Calculation

The ECU calculates the stoichiometric air-fuel ratio dynamically based on the current ethanol percentage:

- **E85**: stoichiometric ratio = 9.85
- **E10**: stoichiometric ratio = 14.13
- A gauge displays the current stoichiometric ratio for the active flex %.

### Short-Term Fuel Trim (STFT)

Short-term fuel trim range limits now respect the AFR target vs current flex % mapping, ensuring the trim does not chase incorrect lambda targets when running high ethanol blends.

### Cranking Fuel Enrichment

Cranking base fuel mass is now adjusted for flex %, providing appropriate cold-start enrichment for ethanol blends that require more fuel to start reliably.

### TPS Cranking Fuel Correction Table Disabled

The "TPS cranking fuel correction %" table was disabled because flood clear functionality already covers that use case.

### Nitrous Controller

The nitrous controller now respects the AFR vs FLEX % conversion when computing target enrichment for nitrous activation.

### CAN Dash (MS Protocol)

The CAN dash MS output now sends the AFR value adjusted for the current flex %, so the dash displays the correct lambda-relative AFR rather than a fixed 14.7-based value.

### AFR Target Page Warning

A warning was added to the AFR Target page noting that the displayed AFR values are lambda-based (14.7 = lambda 1) and are not absolute AFR numbers. The page also displays the current stoichiometric ratio for the active flex blend so the user can interpret targets correctly.

### Timing Table Range

The timing table range was widened to accommodate the advance curves typically needed for higher ethanol content fuels.

### Table Labeling Improvements

- Ignition and fuel switch tables are now labeled explicitly as such rather than the generic "adder" label.
- "Enabled" toggles were made more descriptive.
