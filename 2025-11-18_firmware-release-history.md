# epicEFI Firmware Release History (2025-06 through 2025-08)

*Source: rusEFI Discord, 2025-11-18 | Channel: epicEFI-announcements (1376346219097620492)*
*Contributors: @ggurov, @joesphan*

## Summary

This document consolidates the firmware release announcements posted in the epicEFI announcements channel covering builds from June 2025 through the 8/16 feature release. Each entry includes the download URL and change summary as posted.

## Details

### First epicEFI Firmware Release

> "lets give this a shot, first release of epicEFI firmware"

The initial release of epicEFI firmware was announced. No specific version number was given for the inaugural release.

### 2025-06-15 Build

**Download:** http://content.epicefi.com/firmware/2025-06-15/

Latest build for all target platforms.

### 2025-06-17 Build

**Download:** https://content.epicefi.com/firmware/2025-06-17/

Latest build for all platforms.

### 2025-06-23 Build

**Download:** https://content.epicefi.com/firmware/2025-06-23/

Changes:
- gcc 14.1 toolchain upgrade
- Added TPSPRIME fuel pump enable/disable feature
- Fixed open loop idle vs CLT position table naming and RPM reference

### 2025-07-04 Build

**Download:** https://content.epicefi.com/firmware/2025-07-04/

Changes:
- Smoothed AFR merge/cleanup
- CAN bus IDs for CAN bus data reading
- Small merge

### 2025-07-05 Build

**Download:** https://content.epicefi.com/firmware/2025-07-05/

Changes:
- Merge to commit `085dc0be8d95f1564acb0bd170b65770abd42e2f`
- Fixed acceleration AE being stuck in ms adder mode for new installs

### Beta Bundle

**Download:** https://content.epicefi.com/firmware/BETA_FILES/rusefi_bundle_epicECU.zip

Beta firmware bundle for epicECU, announced by @joesphan.

### Online Flash Tool

**URL:** https://content.epicefi.com/flash/

A new browser-based online flash tool was released, designed to work with any STM32 ECU. This allows flashing firmware without requiring a local desktop application.

### Version 8/6 Features

New in firmware version 8/6:
- Speedometer sweep on ignition-on (mirrors tachometer sweep behavior)
- Non-linear speedometer table for custom gauge scaling
- VSS (Vehicle Speed Sensor) timeout: speed reading will now drop to zero when the VSS signal disappears

### Version 8/14 Features

New in firmware version 8/14:
- **Cruise control** (first draft): activation only via CAN bus / TS buttons; unknown defaults — **use with extreme caution**
- Fixed VVT table error on empty flash

### Version 8/16 Features

New in firmware version 8/16:
- Separate lower wastegate port output
- Haltech dash can now run at any CAN speed (useful for RealDash + SLCAN configurations)
