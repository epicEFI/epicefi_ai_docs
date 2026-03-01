# Real-Time Clock (RTC) Support in epicEFI: Current State and Proposed Solution

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat (1356732771325968630)*
*Contributors: @ggurov, @Joesphan*

## Summary

epicEFI does not have a battery-backed RTC. A GitHub issue was opened to add a "Set RTC" button in the tuning software. ggurov proposed a "set RTC on connect" feature as a practical workaround — the tuning software would automatically push the host PC's clock to the ECU each time a connection is established.

## Details

### No RTC Hardware Battery

The epicEFI hardware does not have an RTC battery, meaning the real-time clock (if present in the MCU) loses its time value when power is removed:

> "there's no rtc battery" — @ggurov
>
> "we don't have rtc" — @Joesphan

### GitHub Issue: "Set RTC" Button

A GitHub issue was created requesting a "Set RTC" button be added to the tuning software UI:

> "they just added an issue to add 'set rtc' button" — @ggurov

This would allow users to manually push the current time from their PC to the ECU when needed.

### Proposed Feature: Set RTC on Connect

ggurov proposed a more automatic alternative — having the tuning software set the ECU's RTC automatically every time it connects, using the host PC's system clock:

> "but if we get phil to add a 'set rtc on connect'" — @ggurov

This would require:
1. The tuning software sending the current timestamp to the ECU as part of the connection handshake.
2. The ECU writing that timestamp to its RTC register at startup/connect time.

This approach removes the need for a battery-backed RTC for basic timekeeping, as the ECU's clock would be re-synchronized on every connection session. The limitation is that any logging done while the tuning software is not connected would have an inaccurate timestamp (starting from an epoch or last-known time).
