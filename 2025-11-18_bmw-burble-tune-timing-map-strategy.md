# BMW Burble Tune: Timing Map Strategy

*Source: rusEFI Discord, 2025-11-18 | Channel: 1389299516808630312 (theory/dev)*
*Contributors: @fast335xi, @joesphan, @ggurov, @mr.zebraaa*

## Summary

Discussion of how BMW "burble" (exhaust pop/crackle on overrun) tuning works at the timing map level, and a broader comparison of aftermarket ECU platforms (BM3, HP Tuners, rusEFI/epicEFI, SpeedyEFI, MaxxECU) in terms of driveability table coverage and tuning philosophy.

## Details

### BMW Burble Timing Map Trick

@fast335xi shared the key technique for getting clean BMW burble behavior:

> "BMW burbles are easy though, trick is to set the timing optimal map to 0 in the high RPM low load range then it'll go exactly to the timing you set in the burble maps."

**How it works:**
- The BMW DME uses a layered timing architecture: a base "optimal timing" map plus one or more "burble" timing offset maps
- In the high-RPM, low-load region (overrun/decel), the optimal timing map value must be set to **0 degrees**
- With the optimal map zeroed out in that region, the burble maps have full authority over ignition timing
- This allows precise control of the timing retard that produces the exhaust burble effect

**Note on table count:**
- BM3 exposes approximately 20 burble-related tables
- Stage X (and other platforms) exposes many more — reportedly around 200 burble tables in a full BMW calibration

### BMW OEM Driveability Table Targeting

@joesphan described targeting BMW OEM driveability tables as a goal for the rusEFI/epicEFI platform:
- This includes replicating OEM idle quality, throttle response, and transient behavior
- Also includes a fuel pressure to deadtime table (for accurate injector timing compensation based on fuel rail pressure)
- The philosophy: "the standalone goal for any brand is to get the car running as factory as possible"

### BM3 Limitations

@fast335xi stopped supporting BM3 after encountering issues with conflicting device stacks:
- A problematic install involved a race box, pedal commander, JB4, and BM3 all installed simultaneously on an X6M
- BM3 was considered too closed a platform
- @joesphan compared BM3's UI to "HP Tuners Jr."

### Platform Comparison

| Platform | Notes |
|---|---|
| BM3 | Closed platform, reflash only, ~20 burble tables exposed |
| HP Tuners | Well-regarded for GM, good table coverage |
| rusEFI/epicEFI | Open, described as "dangerously close to HP Tuners (not in a bad way)", targeting OEM driveability tables |
| SpeedyEFI | More limited table coverage, described as missing tables needed for some setups (e.g., MR2) |
| MaxxECU | Closed, but functional; noted for features like internal outputs and user tables |

@joesphan: "rus is dangerously close to [HP Tuners] — not in a bad way"

@mr.zebraaa noted that rusEFI features like idle-up for fan/alternator control and advanced fuel pump PWM tables were dealbreakers making it hard to go back to simpler platforms.
