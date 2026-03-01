# SD Card Logging Stops When Storage Is Full

*Source: rusEFI Discord, 2026-02-21 | Channel: 1401895238481744052 and 1464230782905352261*
*Contributors: @ggurov, @kostasl, @MykkH*

## Summary

When the SD card fills completely, rusEFI/epicEFI stops writing logs entirely rather than overwriting old data or reserving space. A user also noted that setting the SD logger rate to 1 Hz did not prevent the card from filling up. A potential mitigation — reserving a partition on the SD card — was proposed but not yet implemented.

## Details

### Behavior When SD Card Is Full

When the SD card runs out of free space, the firmware **stops writing to the card**. It does not overwrite old files, rotate logs, or leave a reserved buffer — it simply ceases logging. This was identified as a bug/gap to be addressed in the firmware.

- @ggurov confirmed: "found what happens when it's out of space, gonna address that now"
- @kostasl initially asked whether 64 KB is left unallocated — the answer is no; it stops writing cleanly without reserving space.

### Proposed Fix: Reserve SD Space with a Partition

@kostasl suggested reserving a portion of the SD card at all times using a partition so that the logger always has guaranteed free space to write into. This would prevent an abrupt stop and could allow the system to handle near-full conditions more gracefully.

This approach was under active development consideration as of 2026-02-21.

### Separate Issue: SD Filling Up Despite Low Logger Rate

@MykkH (channel 1464230782905352261) reported their SD card filling up with logs even with the SD logger sample rate set to **1 Hz**. They asked whether there is a way to prevent logging while still using the SD card for tune storage.

- This suggests that even at low sample rates, log files accumulate over time and can fill smaller SD cards.
- No firmware-level log rotation or automatic cleanup was available at the time.
- Practical recommendation: use a larger SD card or periodically clear old log files manually.

### Key Takeaways

- SD logging stops completely when the card is full — no overwrite or rotation behavior.
- A partition-based space reservation was proposed as a fix.
- Even at 1 Hz logging rate, SD cards can fill over time — manage card capacity proactively.
- The firmware-side fix for out-of-space handling was being actively worked on as of 2026-02-21.
