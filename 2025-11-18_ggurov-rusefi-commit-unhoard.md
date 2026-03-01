# ggurov rusEFI Fork: Feature Commit Announcement

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ggurov, @SeV*

## Summary

@ggurov announced a commit to his rusEFI fork that "unhoards" previously private feature work for the community. The commit is large and covers multiple features. Not all of it is expected to be merged into mainline rusEFI immediately, but the code is now publicly available. Separately, @SeV shared a link to a related branch containing CAN-based wheel speed sensor and traction control code for BMW vehicles.

## Details

### Commit Announcement

@ggurov shared the following commit to his rusEFI fork:

**Commit:** `ad41c06e9048a7369ef53df37027b7cd27a04487`
**URL:** https://github.com/ggurov/rusefi/commit/ad41c06e9048a7369ef53df37027b7cd27a04487

@SeV's reaction: "wow, that is huge"

@ggurov on merge expectations:
> "we'll see how much of that gets merged into mainline — but i've completed my obligation to unhoard things and stuff"

### Related: SeV's CAN Wheel Speed / Traction Control Branch

In the same conversation, @ggurov asked @SeV for the four-wheel traction control code, and @SeV shared:

**Commit:** `24b7f540c4e9885674b5b0c762191855f8fdd554`
**URL:** https://github.com/jankovalski/changes_20250203/commit/24b7f540c4e9885674b5b0c762191855f8fdd554

This branch implements BMW ABS CAN message parsing for wheel speeds (see separate doc on CAN wheel speed traction control for details).

@SeV noted the traction control implementation reacts to CAN messages from a BMW traction control/ABS module rather than directly reading four individual wheel speed sensor inputs.

### Context

The commit was part of a broader effort in the epicEFI community to consolidate development work and make features available upstream. Features known to be included in ggurov's fork include: expanded table sizes (32x32 fuel, 20x20 ignition), four ETB pedal maps, CAN button box integration, DFCO toggle, launch control, and various I/O enhancements.
