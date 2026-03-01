# SD Card Free Space Management and Logging Behavior

*Source: epicEFI Discord, 2026-02-20 | Channel: 1401895238481744052*
*Contributors: @kostasl, @ggurov*

## Summary

This thread clarifies how the rusEFI/epicEFI ECU handles SD card free space during continuous data logging. The key threshold is 10 MB of remaining free space, at which point the system should either stop logging or erase the oldest log file to continue.

## Details

### Free Space Threshold

The ECU monitors SD card free space during logging. When free space drops below **10 megabytes**, logging behavior changes:

- @ggurov proposed the rule: "don't keep logging if there's less than 10 megs left"
- @kostasl suggested two possible behaviors at that threshold:
  1. **Stop logging** — cease writing to the SD card.
  2. **Erase the oldest file and keep logging** — circular/rolling log behavior that overwrites the oldest session.

### What the SD Card Space Is Used For

@kostasl's original question highlighted uncertainty about space allocation. The SD card is used for:

- **Continuous data logging** (MLG/CSV format, primary use of space)
- **Error logging**
- **Long-term fuel trim (LTFT) saving**
- **Tune saving**

The answer from the discussion implies that continuous logging can consume all available space unless the 10 MB threshold behavior is properly implemented. The system does not automatically reserve a fixed partition for non-logging data; all functions share the same space pool.

### Practical Recommendation

Users doing extended data logging should periodically offload and clear log files to avoid hitting the 10 MB floor. If the system is configured for rolling deletion (erase oldest), verify that LTFT and tune save files are not at risk of being overwritten.
