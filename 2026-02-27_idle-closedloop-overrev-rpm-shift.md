# Idle Closed-Loop Strategy, Overrev Prevention, and RPM-Based Shift Tables

*Source: rusEFI Discord, 2026-02-27 | Channel: 1470466409602744541*
*Contributors: @ggurov, @Robb235*

## Summary

Three related tuning strategy topics came up during a TCC (Throttle Cable Control) tuning session on a 2000 4Runner running uaEFI. The discussion covered the philosophy of idle closed-loop control (proactively chasing target idle RPM rather than reacting to stalls), using RPM limit tables to prevent overrev, and simplifying gear-change logic with an RPM-based upshift/downshift table.

## Details

### Idle Closed-Loop: Chase the Target, Don't Recover from Stalls

- The recommended approach to idle closed-loop control is to **actively chase the desired idle RPM** at all times.
- The system should continuously adjust (fueling, ignition timing advance, or idle air/throttle position) to maintain target idle RPM.
- This is preferable to a reactive strategy that only acts after RPM has dropped near stall — by that point, recovery may not be possible.
- Key design intent: **prevent the stall condition from ever occurring** rather than attempting to recover from it.

### Overrev Prevention Using RPM Limit Tables

- Robb235 asked whether table data (containing RPM limit information) could be used to prevent engine over-rev.
- Yes: RPM limit data from lookup tables can be applied to configure **rev limiters** within the ECU software.
- The table can encode per-gear or per-condition RPM limits that the ECU enforces via spark cut, fuel cut, or both.

### RPM-Based Upshift/Downshift Table

- Robb235 proposed simplifying the shift logic by using a straightforward **RPM-based upshift/downshift table** instead of more complex multi-variable shift logic.
- A simple RPM table assigns a shift point for each gear (upshift at X RPM, downshift below Y RPM).
- Advantage: easy to calibrate and understand.
- Limitation: a pure RPM-based table does not account for vehicle speed, load, throttle position, or other driving conditions — it may not cover all scenarios effectively (e.g., low-speed high-load lugging vs. high-speed light-load cruising).
- For basic setups, RPM-based shift tables are a practical starting point before moving to more sophisticated multi-axis shift maps.

### Session Context

- Vehicle: 2000 Toyota 4Runner
- ECU: uaEFI running epicEFI firmware
- Tune file at time of discussion: `2000_4Runner_uaEFI_2026-02-27_08.34.02.msq`
- Test log from prior session: `2026-02-26_17.50.52_TCC_Test.zip`
