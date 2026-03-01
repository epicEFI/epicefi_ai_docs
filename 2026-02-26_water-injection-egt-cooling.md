# Water/Methanol Injection Strategies: EGT-Triggered and Decel Cooling

*Source: rusEFI Discord, 2026-02-26 | Channel: 1475534320310554887*
*Contributors: @Joshan_Lu, @Jackscr, @ggurov*

## Summary

Discussion of unconventional water/methanol injection (WMI) strategies: triggering injection based on EGT thresholds, and spraying water during deceleration (DFCO) to cool the engine between gear changes. The decel-spray idea was met with skepticism due to risks of engine stumble, cylinder wall washing, and oil contamination.

## Details

### EGT-Triggered Fuel/Water Injection

- Proposed strategy: configure a trigger so WMI activates when EGT exceeds **900°C**.
- This can be set as a simple threshold condition in the engine management system.
- Context: managing heat in a thermally stressed engine (overheating issues from A/C condenser installation).

### Water Spray During Deceleration (DFCO)

- Proposed: spray water during decel phases to cool the engine between gear changes, using the momentum of the vehicle rather than combustion energy.
- @Jackscr's objections:
  - **Tuning difficulty**: Very small amounts of water cause the engine to stumble and stall at idle.
  - **DFCO interaction**: When DFCO (Decel Fuel Cut-Off) is active, the engine effectively dies — spraying water at this point compounds instability.
  - **Cylinder wall washing**: Water sprayed during decel without full combustion can wash cylinder walls and contaminate engine oil.
  - Pure methanol (rather than water) might be more forgiving in this scenario.
- @Joshan_Lu's counterarguments:
  - The engine is still hot after a pull, so residual heat would vaporize water before it can condense on cylinder walls.
  - Any water/meth remaining in cylinders would be burned off when throttle is reapplied.
  - Pre-turbo injection was discussed as an alternative — cooling the intercooler pre-charge air rather than spraying into the intake post-throttle.

### WMI and Oil Contamination

- Using WMI (especially methanol) already contaminates oil to some degree — this is an accepted trade-off in performance applications.
- Adding water-only injection during decel adds further oil dilution risk without the octane benefit of methanol.

### Overheating Context

- The root cause of thermal issues in this case: installation of a large A/C condenser reduced airflow through the radiator.
- Recommended fix: seal the air path from the front grille through the condenser and radiator to prevent bypass airflow; ensure no gap between condenser and radiator through which air can escape without passing through both cores.
- Longer final drive gearing was also suggested as a way to reduce steady-state RPM (and thus heat) at highway speeds.
