# First-Time ECU Build Experience and Pin Assignment Migration from Mega to Teensy

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @CarolinaCumberland*

## Summary

A community member documented their experience building and tuning an ECU for the first time, including installing knock sensors, creating a base tune from scratch, and validating it over 150 miles of driving. A parallel note covers the process of migrating pin assignments when swapping the controller board from an Arduino Mega to a Teensy.

## Details

### First-Time ECU Build and Base Tune

CarolinaCumberland successfully completed a full ECU installation and tune on their first attempt:

> "Have the knock sensors but never set them. Hell, I am proud of myself. First time on this side of a project. Built my first ECU, created a base tune, and tuned it good enough to make it around 150miles and a lot of idle time."

Key milestones achieved:
- Physically installed knock sensors (not yet configured in firmware)
- Created a base tune from scratch
- Validated the tune through real-world driving (150+ miles) and extended idle sessions

Knock sensor configuration was noted as a next step not yet completed at the time.

### Pin Assignment Migration: Mega to Teensy

When switching hardware from an Arduino Mega-based platform to a Teensy-based board, all pin assignments must be reviewed and updated:

> "Yeah, I am still going through and changing all the pin assignments from the mega"

This is a manual process that requires cross-referencing the pin mapping documentation for both the source (Mega) and target (Teensy) hardware to ensure each function (injectors, coils, sensors, etc.) is mapped to the appropriate physical pin on the new board.
