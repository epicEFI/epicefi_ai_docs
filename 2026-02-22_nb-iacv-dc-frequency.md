# Mazda NB IACV Behavior: DC Duty Cycle and Frequency Settings on epicEFI

*Source: epicEFI Discord, 2026-02-22 | Channel: 1464230782905352261*
*Contributors: @MartyMotorsports, @Neos6443, @ggurov*

## Summary

The Mazda NB Idle Air Control Valve (IACV) behaves counterintuitively: when commanded 0% duty cycle (0 DC), the valve opens rather than closes. This is different from what one might expect in an open-circuit/default-closed scenario. Open circuit and 0 DC are two distinct conditions. A PWM frequency of 250 Hz is recommended to avoid audible noise from the valve.

## Details

### IAC Valve Default Behavior (0 DC)

- When the IACV receives 0% duty cycle, it **opens** — not closes.
- This is a known quirk: the NA and NB Mazda IACVs behave unusually compared to typical solenoid valves.
- Open circuit and a 0 DC signal are two distinct conditions; the valve's response to each is different.
- A misconfigured 0 DC output can cause the idle to jump to ~4000 RPM with the valve fully open.

### Recommended Duty Cycle Starting Points (Mazda NB)

Approximate IAC duty cycle values measured from stock ECU operation (Neos6443, 1.6L NB):

| Target RPM | Duty Cycle |
|------------|------------|
| 900 RPM    | 33.3%      |
| 1100 RPM   | 37.6%      |
| 1250 RPM   | 41.2%      |

Note: Larger displacement engines (e.g., 1.8L) will naturally need higher duty cycles to pass more air at equivalent idle RPM.

### PWM Frequency

- Recommended frequency: **250 Hz** (5V PWM output from ECU)
- At lower frequencies the IACV produces an audible/mechanical noise.
- 240 Hz was also found acceptable on Speeduino (Neos6443) with the same noise suppression result.
- MartyMotorsports reported noise potentially caused by the wideband O2 sensor heater circuit (heater is PWM-controlled; heater activation increased noise level noticeably).

### Firmware Note

- @ggurov recommended loading a development/pre-release epicEFI firmware (from a few hours prior to the conversation) which included additional IACV control options not yet in the public release at that time.
- Users should save their tune multiple times before and after a firmware update to avoid losing calibration data.

### SD LTFT / Tune Saving While Running

- For SD card-based LTFT writes (tune saving while engine is running), use the dashboard named **SD_TUNE_STORE** available from: https://content.epicefi.com/dashboards_publish/
- The relevant firmware settings to enable this feature were being confirmed during this session.
