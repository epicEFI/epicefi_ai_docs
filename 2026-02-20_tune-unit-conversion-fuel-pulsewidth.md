# Tune Issues from Metric-to-US Unit Conversion: Fuel Pulse Width and AFR Settings

*Source: epicEFI Discord, 2026-02-20 | Channel: 1356732771325968630*
*Contributors: @Alice (@__._.alice._.__), @jeybee (@_jeybee)*

## Summary

A tune originally created in metric units was partially converted to US units to improve readability for other users. This mixed-unit state introduced a bug causing incorrect fuel base reference pulse width values and unexpected AFR behavior. The root cause was identified as the unit conversion process corrupting or misrepresenting certain tuning parameters.

## Details

### Fuel Base Reference Pulse Width Error

- @jeybee identified that the **fuel base reference pulse width** in Alice's tune was set incorrectly.
- The error is suspected to stem from the tune being originally configured in **metric units**, then having some fields changed to **US units** without a clean full conversion.
- Alice confirmed: "I think it's because I made this originally in metric. But changed some units to US units to make it easier for you all... but now I'm suffering the consequences."

### AFR Setting Uncertainty

- Alice was considering changing an AFR-related value from **360 to 370**.
- Concern raised: the direction of effect was uncertain — changing from 360 to 370 was speculated to potentially make the mixture **more lean**, though this was not definitively confirmed.
- @jeybee suspected a **bug** exists in the system related to how this value is handled after the unit change.

### Lessons Learned

- Mixing metric and US unit settings within a single tune can cause parameters to be interpreted incorrectly by the firmware.
- It is safest to choose a unit system at tune creation and maintain it consistently throughout — mid-session unit conversions on individual fields risk corrupting dependent calculations.
- If a unit change must be made, all related parameters should be reviewed and verified rather than converting fields individually.
