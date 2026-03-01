# STM32H7 Dual-Core MCU Usage in epicEFI: M7 Only, M4 Unused

*Source: rusEFI Discord, 2025-11-20 | Channel: 1401895238481744052*
*Contributors: @ggurov (ggurov), @MykkH (mykkh)*

## Summary

The STM32H7 series contains a dual-core ARM processor (Cortex-M7 + Cortex-M4). The epicEFI/rusEFI firmware currently uses only the M7 core; the M4 co-processor is not utilized. At least one community hardware builder (maxx) is using a dual-core H7 MCU, but firmware support for the second core does not exist yet.

## Details

### Hardware Context

User MykkH asked whether epicEFI firmware on the STM32H7 utilizes the M4 co-processor or only the M7.

ggurov confirmed:
- The firmware **only uses the H7 (M7) side** and ignores the F4/M4 core entirely.
- According to the lead developer (Andrey), H7 support itself was only added **very recently** — the H7 was not a supported target until shortly before this discussion.
- The necessary skills/bandwidth to implement dual-core operation have not been available yet.

### Hardware Models

The dual-core STM32H7 MCUs being referenced are:
- **STM32H743** (Cortex-M7 @ 480 MHz + Cortex-M4 @ 240 MHz, no crypto)
- **STM32H753** (same as H743 plus hardware crypto/hash)

Both are used interchangeably in this context. ggurov noted that community member "maxx" is building hardware around one of these dual-core variants.

### Implications

- Any epicEFI board based on STM32H743/H753 currently operates as a single-core M7 design, regardless of the M4's presence.
- Future firmware development could leverage the M4 for dedicated tasks (e.g., CAN handling, sensor processing), but this is not implemented.
- For current builds, the choice between H743 and H753 is effectively a feature parity choice (crypto/hash), not a performance consideration within epicEFI.
