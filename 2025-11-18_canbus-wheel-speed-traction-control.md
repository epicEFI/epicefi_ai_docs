# CAN Bus Wheel Speed Sensors for Traction Control

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ToyotaTerrorist, @ggurov, @Lysdex, @SeV*

## Summary

Discussion about using CAN-bus-sourced ABS wheel speed data to implement traction control in rusEFI/epicEFI. Both Ford Falcon and Audi vehicles broadcast all four wheel speeds from their ABS modules over CAN bus. A code branch by @SeV (jankovalski) implements BMW E46 ABS CAN message parsing for wheel speeds in rusEFI. The current mainline rusEFI traction control implementation is limited to approximately two wheel speed sensor inputs; the CAN-based approach is the recommended path to achieving full four-wheel traction control. The BMW mk60 ABS module was highlighted as a particularly accessible option for adding standalone traction control via CAN.

## Details

### Vehicles That Broadcast Wheel Speeds Over CAN

Confirmed vehicles that transmit all four wheel speeds from the ABS module over CAN bus:

- **Ford Falcon** — all 4 wheel speeds via ABS
- **Audi** (unspecified models) — all 4 wheel speeds via ABS
- **BMW E39** — all 4 wheel speeds plus traction torque reduction request (@SeV)
- **BMW E46** — all 4 wheel speeds, parsed in the code snippet below

> "My favourite thing I found out the other day is both the Falcon and the Audi broadcast all 4 wheel speeds from the ABS over CAN bus" — @ToyotaTerrorist

> "nah, just stock e39 abs, not sure the model but it's sending all 4 wheel speeds and traction torque reduction request over can" — @SeV

### Current rusEFI Traction Control Limitation

The current mainline rusEFI implementation is limited in how many wheel speed sensors it can process simultaneously:

> "can barely take in 2 sensors" — @ggurov (referring to direct hardware inputs)

The CAN bus approach bypasses this hardware input limit by receiving all four speeds as a single decoded CAN message.

### BMW E46 ABS CAN Parsing — Code Reference

@SeV (jankovalski) shared a code branch implementing BMW E46 ABS CAN parsing for wheel speeds in rusEFI. The commit is at:
https://github.com/jankovalski/changes_20250203/commit/24b7f540c4e9885674b5b0c762191855f8fdd554

Source file: `can_vss.cpp`

Relevant parsing snippet:
```cpp
float processBMW_e46(const CANRxFrame& frame, efitick_t nowNt) {
    engine->outputChannels.vehicleSpeedFrontLeft = (getTwoBytesLsb(frame, 0) - 44) * 0.063003631;
    engine->outputChannels.vehicleSpeedFrontRight = (getTwoBytesLsb(frame, 2) - 44) * 0.063003631;
    engine->outputChannels.vehicleSpeed...
```

The formula `(raw_value - 44) * 0.063003631` converts the raw two-byte little-endian CAN value to vehicle speed (units unspecified in the snippet, likely km/h or m/s). The offset of 44 removes a zero-point bias in the BMW ABS encoding.

Note: @SeV stated this was a different approach to the 4-wheel traction control and had not yet been tested in a vehicle at the time of the discussion.

### ggurov's Planned Four-Wheel Traction Control

@ggurov described his intended traction control setup:
- Start with 0% slip allowed (tightest setting)
- Use a CAN button box to switch to progressively more permissive slip targets on demand ("make it spicy")
- Requires a quad wheel speed signal generator for bench testing before deployment

Development status: @ggurov needed to write an Arduino-based test harness with four independent PWM generators and potentiometers to simulate adjustable wheel slip before finalising the traction control maths.

> "i need to write something on an arduino to generate pwm that kinda looks like wheels to test maths and stuff — 4 generators, and pots for adjustable slip — so i can generate it, measure it, and verify maths" — @ggurov

### BMW mk60 ABS Module as a Standalone Traction Control Option

@Lysdex highlighted the BMW mk60 ABS module as a practical approach for vehicles that do not natively broadcast wheel speeds over CAN:

- The **mk60e5** variant performs traction control autonomously without requiring any ECU involvement.
- Can purchase the M3 variant and flash it with a motorsport firmware that exposes tunable traction control parameters.
- Outputs a single CAN message with traction torque reduction requests, which the ECU can read.
- This approach offloads the traction control calculation to the ABS module, simplifying ECU integration.

> "the mk60e5 does traction control as standard — no ecu required — can buy the m3 version and flash it with a motorsport firmware — then you can adjust all of the params" — @Lysdex

### Traction Control Algorithm Concepts Discussed

@Lysdex described fundamental traction control logic:
- You do not necessarily need to know which specific wheel is slipping.
- A simpler approach: detect that *any* wheel is slipping and reduce torque.
- Or use front vs. rear wheel speed differential — this is common in drag racing applications.
- More sophisticated four-wheel slip detection requires knowing individual wheel speeds.

### Traction Control Application: Ford Falcon Street Use

@ToyotaTerrorist's specific use case — using the Falcon's OEM ABS CAN data to implement street traction control:
> "So I can set up really nice traction control on the Falcon for street duties — unlike Ford's factory style that'll kill me lol"

@ggurov noted that there is existing four-wheel traction control code that had not yet been merged into the mainline rusEFI repository (referred to as "hoarded").
