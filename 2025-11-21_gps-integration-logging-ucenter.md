# GPS Integration, Logging, and u-center Configuration

*Source: rusEFI Discord, 2025-11-21 | Channel: general-tuning (1401895238481744052)*

*Contributors: ggurov, Joesphan, fast335xi*

## Summary

epicEFI supports GPS modules connected over serial (UART). When GPS is enabled, all `gps_` variables (including latitude, longitude, date/time) are automatically included in datalogs. The default tested module is the GY-NEO6MV2 (u-blox NEO-6M). To increase update rate beyond the default (typically 1 Hz), the u-blox receiver must be reconfigured using the u-center application. Users noted that 10 Hz is generally sufficient for most logging; 25 Hz is desirable for performance metrics. The g-mouse USB GPS dongles contain u-blox chips with UART pads accessible by soldering to the edge connector.

## Details

### Automatic GPS Variable Logging

- When GPS is enabled in epicEFI, logs **automatically** include the `gps_` variable set.
- Variables include GPS latitude, longitude, and date/time from the GPS receiver.
- This makes GPS-based lap timing and route replay from standard datalogs possible without extra configuration.
- ggurov: *"with the GPS, logs automatically log the gps_ variables — so it's got the date/time in that"*

### Supported Hardware

- Primary tested module: **GY-NEO6MV2** (u-blox **NEO-6M** chip).
- ggurov also tested a u-blox **7** variant (inside a "g-mouse" USB dongle). The g-mouse module was found to default to USB-only output; the serial/UART output required reconfiguration via u-center or was not accessible without soldering.
- For the g-mouse dongle, the edge through-hole pads are labeled **E V R T G P** (VCC, RX, TX, GND, PPS; E unknown). Soldering to these pads gives serial access.
- A separate serial port is available on the epicEFI/mega_epic system for expansion.

### GPS Update Rate Configuration

- Default u-blox modules output at **1 Hz**. This is too slow for meaningful vehicle performance data.
- To increase update rate, use **u-center** (u-blox configuration software): [Tutorial video](https://youtu.be/boPIh0o7_dg) and [another reference](https://youtu.be/GLtEtxPWoIk).
- The u-blox 7 chip can reportedly reach **20 Hz** with a u-center configuration update.
- Joesphan: *"you have to run a config with u-center to get it up to 20 Hz"*
- Users must also set the **baud rate** in epicEFI firmware to match what is configured in the GPS module.
- ggurov: *"mainly set baud rate, and configure gps itself via ublox"* — some customization per module is required.

### Update Rate Recommendations

- **10 Hz**: Sufficient for general logging (ggurov tested, confirmed workable).
- **25 Hz**: Preferred for performance metrics such as 0–60, 60–130 timers (fast335xi: *"25 Hz is almost necessary for performance metrics"*). The Dragy Pro reportedly uses a 25 Hz u-blox chip.
- **1 Hz** (default): Adequate for route logging but not useful for acceleration measurement.
- A $10 10 Hz module was noted as sufficient for most use cases; high-end 25 Hz modules (e.g., multi-constellation, 92-channel) cost significantly more (~$80+).

### Google Maps URL Integration

- The `gps_lat` and `gps_long` log variables can be used to construct Google Maps URLs.
- Single point: `https://www.google.com/maps?q=<lat>,<long>`
- Multiple points (route): use the `/dir/` path format with multiple `<lat>,<long>/` waypoints. This produces a long URL but is supported.
- ggurov verified the multi-point approach works from log data.

### Known Limitations / TODOs

- Some GPS modules (g-mouse with u-blox 7) may default to USB-only; serial reconfiguration via u-center may be needed.
- Baud rate must be matched between firmware and GPS module configuration.
- 0–60 / 60–130 dashboard timers were proposed by fast335xi as a future feature using GPS data.
