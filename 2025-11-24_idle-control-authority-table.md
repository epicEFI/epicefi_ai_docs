# Idle Control Authority Table — CLT vs Target RPM

*Source: rusEFI Discord, 2025-11-24 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Joesphan (@joesphan), Tera (@teraflopping)*

## Summary

Explains the idle control authority table in rusEFI/epicEFI, which defines the percentage of maximum throttle authority the idle controller is permitted to use at each coolant temperature and target RPM combination. Also covers the "force into idle" feature that prevents rev hang above the idle RPM range.

## Details

### Idle Authority Table

The idle control table axes are:

- **X axis:** Coolant temperature (CLT)
- **Y axis:** Target idle RPM

The **cell value** is the idle authority percentage — it represents how much of the configured maximum idle throttle authority (set elsewhere as a % of TPS travel) the idle controller is allowed to use at that operating point.

Example: If the maximum idle authority is set to 15% TPS, and the table cell reads 50%, the idle controller can use up to **50% of 15% TPS = 7.5% TPS** at that CLT/RPM point.

Setting all cells to 50% is a neutral starting point. Values can be trimmed live while the engine is running to reduce or increase idle air authority as the engine warms up.

### Recommended Live Tuning Procedure

1. Start the engine and allow it to reach operating temperature.
2. Observe actual idle RPM vs target RPM.
3. If idle is running high, reduce the authority value for the current CLT/RPM cell (e.g., try 40% instead of 50%).
4. Changes must be made **live** (engine running) to observe their effect. Idle parameters change as the engine warms up, so tune at multiple temperature points.
5. Idle air authority also interacts with idle timing control — both can be used to trim idle speed.

### Force-Into-Idle Feature

rusEFI includes a feature (developed collaboratively, not present in FOME) that forces the engine into idle mode when throttle is released to a position just above the idle RPM range threshold. This prevents **rev hang** — the condition where RPM hangs above idle after lifting off the throttle.

The feature detects that the throttle has been dropped to near-idle and actively commands the idle controller to take over, pulling RPM down to target. This is most noticeable as an improvement when transitioning from part-throttle to idle at operating temperature.

### B58S Injector Configuration Note

The newer BMW B58S engine (as fitted in later M vehicles and the Supra A91) uses a **dual injection system** with **12 injectors total**:

- 6 Direct Injection (DI) injectors
- 6 Port Injection (PI) injectors

When configuring fuel injection for a B58S swap, both injector banks must be configured. There is a toggle in rusEFI to disable DI and run PI-only if needed for initial bring-up or testing.
