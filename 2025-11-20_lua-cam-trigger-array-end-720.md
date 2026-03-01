# Lua Cam Trigger Configuration: Array Must End at 720 Degrees

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov, Tera (teraflopping)*

## Summary

When configuring a cam trigger pattern in Lua for epicEFI, the trigger definition array must end at exactly 720 degrees (one full engine cycle for a 4-stroke). The firmware will report an error if the array ends at any other value (e.g., 621 degrees). The fix is to shift the entire trigger array forward so the last entry lands on 720.

## Details

- ggurov encountered this while adapting a Lua cam trigger example (sourced from the FOME/rusEFI repository) for use with epicEFI.
- The firmware performs a validation check on the trigger array and requires the final entry to be **720 degrees**.
- Error encountered: firmware reported the array "had to end on 720" (not 621 or any other value).
- **Fix applied**: Shift the entire trigger angle array so the last value equals 720. This preserves the relative spacing between teeth while satisfying the firmware's end-point requirement.
- This requirement is consistent with FOME behavior but the error message could be clearer — Joesphan noted: "that error message needs to make more sense."

## Notes

- The cam trigger Lua code path used here was taken from a working FOME repository example and confirmed to be the same code Tera was actively using.
- If adapting cam trigger definitions from other ECU platforms, ensure the angle array spans a full 720-degree engine cycle and terminates at 720.
