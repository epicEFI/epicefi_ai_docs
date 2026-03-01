# TCU/TCC Control Tuning for A340 Transmission in epicEFI

*Source: epicEFI Discord, 2026-02-26 | Channel: 1470466409602744541*
*Contributors: @ggurov, @Robb235, @GGGI, @DiMo, @Joesphan, @SanGawku, @ZeeKay, @punisher496, @Ramzes*

## Summary

A live development and tuning session documenting the epicEFI/rusEFI TCU (Transmission Control Unit) feature being tested on a vehicle with an A340 automatic transmission. @Robb235 successfully completed a 2-hour drive with the TCU controlling the Torque Converter Clutch (TCC) using epicEFI standalone — no longer tethered to the factory ECU. The session covers TCC solenoid wiring, PWM frequency requirements, the `isTccAllowed()` logic, load/TPS table configuration, lock/unlock speed control, and a firmware off-by-one bug fix in gear indexing.

## Details

### TCC Solenoid Assignment

- The TCC solenoid must be assigned in the dedicated TCC solenoid configuration dialog within the ECU software — not treated as a generic solenoid.
- In this session, the TCC solenoid was initially unassigned (causing TCC not to engage), then correctly assigned.
- @Robb235 was using the A340 with TCC controlled as solenoid #3 on pin D43 of a Mega-based board.

### Firmware Specific to uaEFI with TCU Code

- There is firmware specific to the uaEFI hardware that includes TCU code.
- TCU functionality is being enabled for all Proteus ECUs as well.

### PWM Frequency for TCU Solenoids

- Question raised: how fast does the PWM signal need to be for a TCU solenoid, and whether PWM over CAN bus is feasible.
- For the A340 transmission (ECU controlling line pressure), the required PWM frequency is approximately **330 Hz**.
- Driver ICs discussed for this purpose: **BSP78** and **VNLD5160**.
- In @Robb235's case, TCC ran successfully as On/Off (no PWM) and engagement was smooth enough.

### TCC Request: Duty Cycle vs On/Off

- Instead of listening to the ECU's on/off signal to determine TCC state, the preferred approach is to request the **TCC duty cycle** directly from the ECU on a dedicated output channel.

### isTccAllowed() Logic

The TCC lockup permission function (`isTccAllowed(gear)`) checks the following conditions in order:

```cpp
auto load = getFuelingLoad();
if (load < config->tcu_tccMinLoad[static_cast<int>(gear)+1]) {
    return false;
}
if (load > config->tcu_tccMaxLoad[static_cast<int>(gear)+1]) {
    return false;
}
// TPS check follows similarly
```

- **Load source**: Uses `getFuelingLoad()` (i.e., `fuelload`), which maps to MAP on MAP-based tunes and TPS on alpha-N tunes.
- If TPS (via `DriverThrottleIntent` sensor) is less than the configured minimum TPS for that gear, TCC lockup is **not allowed**.
- Speed is not checked within `isTccAllowed()` — speed-based lock/unlock is handled separately.

### Load and TPS Table Configuration

- To make load effectively a non-factor (letting TPS and speed drive TCC decisions):
  - Set **Min Load = 0**, **Max Load = 600** (or a high value) — this always passes the load check.
  - Setting Min Load = 600 would effectively disable TCC lockup entirely (load never reaches minimum).
- A **Min/Max TPS** pair was added alongside Min/Max Load so users can choose TPS as the primary controlling variable.
- Consensus: load source should be user-selectable (MAP, TPS, cylinder fill %, etc.) — logged as a feature request.

### TCC Lock/Unlock Speed Control

- The `tcc lock/unlock speed` setting controls at what vehicle speed TCC engages and disengages.
- The lock/unlock speed table is structured per-gear, similar to the automatic shift points table.
- Proposed 3D table structure (feature request):
  - X-axis: Gear
  - Y-axis: TPS %
  - Z (value): Lock speed
  - Z2 (second table): Unlock speed
- This would allow speed-dependent TCC behavior (e.g., looser unlock threshold at 75 mph vs. 45 mph in 4th gear).
- Alternative proposal: integrate TCC lock/unlock entries directly into the auto shift schedule alongside gear shift rows.

### TCC Lockup Gear Restrictions

- @Robb235: "Basically should only lock up in 4th gear per the big table. And always unlocked if throttle is closed per the small table."
- @DiMo: "Don't allow TCC lockup in gears 1–2, even 3 really."
- Factory behavior on some vehicles (e.g., Chevy Lumina): TCC unlocks when throttle is released while in 4th gear at highway speed.
- Setting Min TPS % to 0 will keep TCC locked even with throttle closed.

### Coasting Behavior

- When coasting down in a locked gear, the TCClock should hold until vehicle speed drops below a configured speed threshold, then disengage.
- This threshold is separate from the full lock/unlock speed table.

### Firmware Bug: Off-By-One in Gear Indexing

- A bug was identified where TCC lock/unlock did not follow the configured speed rules.
- Root cause: gear array starts at index -1 (to account for Reverse), causing gear reads to be off by one.
- Fix applied:
  ```cpp
  bool solenoidState = config->tcuSolenoidTable[i][static_cast<int>(gear) + 1];
  ```
- After this fix, TCC correctly locked and unlocked per the configured speed thresholds.

### TCC Unlock on Throttle-Off: OEM Behavior

- On many OEM systems, TCC unlocks when the driver lifts off the throttle at highway speeds.
- Rationale: OEM converters are not designed to handle large amounts of engine braking torque through the TCC.
- GM 4L80 (diesel application): TCC stays locked unless RPM drops sufficiently or hard throttle is applied — behavior varies by vehicle.
- For tuning purposes, mimicking factory behavior is a sensible starting point; users can adjust Min TPS % to tune unlock aggressiveness.

### Broader TCU Platform Notes

- The rusEFI/epicEFI TCU currently handles solenoid control only (no torque management).
- A uaEFI-based standalone TCU is estimated at ~$200 and is viable for torque-converter automatic transmissions.
- This TCU is **not** related to the 8HP (ZF 8-speed) implementation.
- GM 4L60/4L80 transmissions use PWM for pressure control — noted as compatible with this approach but requiring sufficient I/O.
- Useful external reference for 4L60/65E transmission tuning: https://ls1tech.com/forums/automatic-transmission/1416477-how-4l60-65e-trans-tuning-shifting-tcc-tm-w-pics.html
