# Proteus Flash Space Management: Removing Unused Lua Lookups

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov*

## Summary

The Proteus ECU has limited flash storage. When flash space is exhausted, the firmware will fail to build or function correctly. A known contributor to excessive flash usage is unnecessary Lua lookup tables compiled into the firmware. Removing unused Lua lookups freed enough flash to resolve the issue.

## Details

- Flash space on Proteus is constrained. If a firmware build runs out of flash, features must be trimmed.
- "Lua lookups" in the epicEFI/rusEFI context refers to interpolation table structures used by Lua scripts that are compiled into the firmware binary (not just loaded as scripts).
- ggurov removed several lookup tables that were not needed for the active configuration, resolving the out-of-flash-space condition.
- This is a recurring consideration when adding features to Proteus firmware: each additional table, lookup, or compiled Lua element consumes flash.
- For reference, the GDI-4 firmware binary is small (text: 28936 bytes, total ~49 KB) and has significant headroom, but the full Proteus firmware build is much larger and closer to the limit.

## Related

- If building custom Proteus firmware, audit all Lua lookup tables in the build configuration and remove any that are not used by the active tune.
- Flash usage can be checked by examining the `.map` or `.elf` build output for section sizes.
