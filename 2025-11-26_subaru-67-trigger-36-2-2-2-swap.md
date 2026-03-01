# Subaru 6/7 Trigger Wheel vs. 36-2-2-2 Swap on GC8 WRX

*Source: rusEFI Discord, 2025-11-26 | Channel: 1356732771325968630*
*Contributors: DCwerx (@dcwerx), Joshan Lu (@joshanlu)*

## Summary

Discussion during a live epicEFI E840 install on a Subaru GC8 WRX. The 6/7 trigger pattern (OEM Subaru) is confirmed to work with rusEFI after community verification, but is historically problematic on other standalone ECUs — leading most tuners to swap to the 36-2-2-2 trigger wheel as a workaround.

## Details

**Context:**
A Subaru GC8 WRX was being fitted with an epicEFI E840 ECU for a car that needed to compete in both autocross and drag racing over the same weekend.

**Trigger wheel situation:**
The GC8 WRX uses a 6/7 tooth trigger pattern (OEM Subaru reluctor wheel). This is a known problematic trigger on many standalone ECU platforms.

**Confirmed working with rusEFI:**
After searching the rusEFI online documentation and forums, DCwerx confirmed that the 6/7 trigger pattern is supported and documented as working with rusEFI. The install proceeded without a trigger wheel swap.

**Common workaround (non-rusEFI):**
On other platforms (Speeduino, MicroSquirt), the 6/7 trigger frequently causes issues and the standard remedy is a full trigger wheel swap to 36-2-2-2, which requires a timing belt job. This is unnecessary when using rusEFI if the 6/7 decoder is configured correctly.

**Practical note:**
Most Subarus that end up on aftermarket standalone ECUs retain the OEM 6/7 trigger. Newer models that already have a 36-2-2-2 trigger are typically vehicles that came with a factory programmable ECU and are therefore less likely to need standalone EFI in the first place.
