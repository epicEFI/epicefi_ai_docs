# epicefi_ai_docs

AI-generated technical documentation extracted from the **epicEFI Discord** community. Raw support threads and troubleshooting discussions are periodically processed by Claude and turned into clean, searchable markdown articles.

## Document Format

Files are named `YYYY-MM-DD_short-topic-description.md`. Each document includes a **Summary**, **Details**, and **Notes** section, plus source metadata (Discord channel and contributors).

---

## Articles

### 2026-02-20

- **[AC Pin Labeling: STM vs uaEFI Names](2026-02-20_ac-pin-labeling-stm-vs-uaefi.md)** — In 2026-01-06 firmware, AC request and AC relay pins are labeled using STM MCU pin names rather than uaEFI board-level names, which can cause confusion when mapping in TunerStudio.
- **[CAN Bus Solenoid Control via MOSFET on uaEFI](2026-02-20_canbus-solenoid-mosfet-uaefi.md)** — Solenoid control over CAN between a microcontroller and Mega2560, with options for direct PWM on the main board or CAN-mirrored PWM via mega_epic firmware.
- **[Cranking Injection Sync Modes](2026-02-20_cranking-injection-sync-modes.md)** — Batch, sequential, and simultaneous injection modes during cranking; wasted/batch fires until full sync is attained before switching to true sequential.
- **[DriverThrottleIntent Virtual Sensor](2026-02-20_driverthrottleintent-sensor.md)** — A virtual sensor combining TPS and PPS data that adapts automatically for ETB or cable-actuated throttle configurations.
- **[Engine Stats Persistent Storage via SD Card](2026-02-20_engine-stats-persistent-storage.md)** — Feature interest in tracking persistent engine stats (max PSI, max RPM, rev limiter count, miles driven) using the SD card for non-volatile storage.
- **[epicECU SPI EGT Channels](2026-02-20_epicecu-spi-egt-channels.md)** — The epicECU hardware supports 8 SPI-connected EGT channels for per-cylinder or per-bank exhaust temperature monitoring.
- **[epicEFI Serial/USB CRC Protocol](2026-02-20_epicefi-serial-usb-crc-protocol.md)** — CRC mode must be activated (F then S) before the ECU responds to commands; subsequent commands are wrapped in CRC frames with a 2-byte length and 4-byte CRC32.
- **[EpicTuner Autotune and Remote Features](2026-02-20_epictuner-autotune-remote-features.md)** — Autotune and remote access in EpicTuner are gated behind the `EPICTUNER_FEATURES` environment variable.
- **[Electric Turbo/Supercharger Build: 48V, 238 CFM](2026-02-20_eturbo-supercharger-boost-power.md)** — Community integration of a 48V electric compressor targeting 238 CFM at 8–10 psi boost, engaging above 4,000 RPM via a 48V regulator.
- **[Firmware 2.20: Cranking Ignition Timing Fix](2026-02-20_firmware-220-cranking-ignition-fix.md)** — Version 2.20 (2026-02-20) fixes a cranking ignition timing bug that caused the ignition event to fire one engine cycle too early.
- **[IGN1A Coil: Identification and Sourcing](2026-02-20_ign1a-coil-identification-sourcing.md)** — The IGN1A coil originates from Mercury Marine Optimax outboards; Yunyi clones (~£22) are considered better quality than branded resellers (~£73).
- **[Injector Pulse Width and VE Table Resolution](2026-02-20_injector-pw-ve-resolution.md)** — Injector PW steps in 0.0033 ms increments, requiring ~0.16–0.2 VE table units to advance to the next available step.
- **[Kill Switch Disconnecting High-Side Coil Power](2026-02-20_kill-switch-power-bus-ignition.md)** — When a kill switch disconnects 12V from coil high sides, the ECU's ground-switching outputs become ineffective and the starter solenoid cannot engage.
- **[OEM Torque Management for Automatic Transmissions](2026-02-20_oem-torque-management-auto-transmission.md)** — OEM ECUs use a torque-demand architecture for gear shift torque reduction, with spark retard as the primary mechanism.
- **[PWM Capabilities: Arduino Mega vs ESP32](2026-02-20_pwm-microcontroller-comparison-esp32.md)** — ATmega2560/328P are limited to fixed-frequency hardware PWM; the ESP32-S2 supports flexible PWM across up to 8 pins from sub-10 Hz to MHz.
- **[RPM Instability and Trigger Wheel Filtering](2026-02-20_rpm-instability-filtering-trigger-wheel.md)** — Erratic RPM under throttle can be stabilized by enabling RPM filtering; this setup uses twin optical sensors for sync and position.
- **[SD Card Free Space Behavior Below 10 MB](2026-02-20_sd-card-logging-free-space.md)** — When SD card free space drops below 10 MB, the ECU should stop logging or erase the oldest file.
- **[SD Card: Real-Time Tune Writes and Power-Loss Safety](2026-02-20_sd-card-logging-realtime-tuning.md)** — SD card support enables live tune writes while running, surviving sudden power loss; LTFT corrections persist across power cycles.
- **[SD Card Setup and Troubleshooting](2026-02-20_sd-card-setup-troubleshooting.md)** — SD card requires FAT32 format and "SD Card Logging: True"; workaround involving an external trigger loop resolves combined SD/flash issues.
- **[SD Card Tune Storage and Boot Selection](2026-02-20_sd-card-tune-storage-boot.md)** — The ECU selects the tune with the highest writeId at boot; real-time changes are written to the SD card first for power-loss recovery.
- **[STM32H753: Not Recommended for Custom PCB Development](2026-02-20_stm32-h753-custom-pcb-io-limits.md)** — The STM32H753 Nucleo-144 runs epicEFI but is not suitable for custom ECU work; the DCwerx STM32-H7 Adapter Board is the preferred alternative.
- **[Tune Unit Conversion Bug: Fuel Pulse Width](2026-02-20_tune-unit-conversion-fuel-pulsewidth.md)** — A partial metric-to-US unit conversion introduced incorrect base reference pulse width values and unexpected AFR behavior.
- **[Turbo BOV, Spool Lag, and ETB Strategies](2026-02-20_turbo-bov-spool-lag-etb.md)** — Overview of BOV function, spool lag, surge, and ETB strategies for managing compressor surge and idle on turbocharged engines.
- **[Turbo Surge Types and BOV Protection](2026-02-20_turbo-surge-types-bov-protection.md)** — On-throttle vs off-throttle surge mechanics, damage modes, and the importance of a MAF sensor for fueling stability on boosted engines.

### 2026-02-21

- **[B48/B58 Brushless Fuel Pump and FPR Swap](2026-02-21_b48-b58-brushless-pump-fpr-swap.md)** — B48/B58 use an EKP-controlled brushless pump that can be deleted and replaced with a Walbro 525 and aftermarket FPR for standalone ECU use.
- **[Boost Tuning: Lean AFR, Fuel Cut, and Rolling Launch Setup](2026-02-21_boost-tuning-afr-rolling-launch.md)** — Lean AFR (14.4:1) under boost triggered fuel cut codes; required setting up a rolling launch to safely manage boost transitions.
- **[CAN Output Lua Counter for Knock/Proteus (Subaru EZ30)](2026-02-21_can-output-lua-counter-knock-proteus.md)** — How to replicate a 0–255 incrementing counter at 20 Hz on CAN using Lua scripts and math channels.
- **[Crank/Cam Sensor Sync: Edge Selection Fix](2026-02-21_crank-cam-sensor-sync-troubleshooting.md)** — Trigger sync errors on a B-series engine were resolved by flipping the edge selection on the crank input.
- **[DC1 vs DC2 for VW Electronic Wastegate Control](2026-02-21_dc1-dc2-ewg-control.md)** — The DC1 output on the bipolar pair is significantly better suited than DC2 for controlling a VW EWG on uaEFI.
- **[DFCO Configuration: MAP and TPS Conditions](2026-02-21_dfco-configuration-map-tps.md)** — DFCO triggers on TPS/APP and RPM; MAP-based DFCO can be achieved by assigning MAP and TPS to the same physical pin as a workaround.
- **[EGT Sensors: Useful for Safety, Not Primary Fueling](2026-02-21_egt-sensor-tuning-limitations.md)** — EGT sensors cannot be used for primary AFR tuning because the AFR-vs-EGT curve is V-shaped; useful for safety monitoring and cylinder balance.
- **[epicEFI Display: MIPI vs HDMI](2026-02-21_epicefi-display-mipi-hdmi.md)** — HDMI displays work out of the box with stock epicEFI images; MIPI displays require additional driver support not present in the base image.
- **[epicFW/uaEFI Reconnect Bug Fix](2026-02-21_epicfw-uaefi-connection-fix.md)** — A bug on uaEFI boards causes TunerStudio to fail reconnection without power-cycling; fix is a full erase and flash with the .hex file.
- **[Fuel Trim, Wall Wetting, and MAP Multiply](2026-02-21_fuel-trim-wallwetting-map-multiply.md)** — Wall wetting compensation and the "map multiply" feature are common causes of unexpectedly small or unstable fuel adjustments during tuning.
- **[Nitrous Staging at 100%: Injector Bug](2026-02-21_nitrous-staging-100pct-injector-bug.md)** — A bug causes all eight injector outputs to behave erratically when transitioning from 99% to 100% nitrous staging.
- **[SD Card Full: Logging Stops Completely](2026-02-21_sd-card-logging-full-stop.md)** — When the SD card fills, epicEFI stops writing logs entirely; a partition-based space reservation is proposed as a fix.
- **[SD Card and Flash Write Behavior](2026-02-21_sdcard-flash-write-behavior.md)** — epicEFI uses the last SD card partition with a write-ID arbitration system; STM32 flash has ~10,000 write cycles per page.
- **[STFT Behavior with "Stop STFT for Rot Idle" Disabled](2026-02-21_stft-rot-idle-behavior.md)** — Setting "Stop STFT for Rot Idle" to False causes STFTs to remain active when Rot Idle is disabled.
- **[TCU Solenoid Wiring and Gear Selector Integration](2026-02-21_tcu-solenoid-gear-selector-wiring.md)** — TCU integration details including EPC/TCC duty parameters and high-side driving requirements for transmission solenoids via a custom conditioner board.
- **[uaEFI PCB Design Quality: Via Current and ETB Concerns](2026-02-21_uaefi-pcb-design-quality-notes.md)** — Community discussion on uaEFI via current capacity for high-current loads like ETB, with recommendations for future single-sided PCB designs.

### 2026-02-22

- **[Crank Sensor Input Types: Hall vs VR](2026-02-22_crank-sensor-hall-vr-input.md)** — epicEFI has no generic "Crank Input" — Hall-effect sensors use Hall inputs and VR sensors use VR inputs; the electrical type determines the pin.
- **[Crank Signal Present But No RPM on 36-2 Wheel](2026-02-22_crank-sync-no-rpm-36-2.md)** — No RPM display despite a received crank signal was caused by trigger phase overrides being disabled in the tune.
- **[Fuel Pressure Units: PSI vs kPa Entry Bug](2026-02-22_fuel-pressure-units-kpa.md)** — Entering fuel pressure in PSI instead of kPa caused extremely rich running; a UI improvement was requested to add unit labels.
- **[Arduino Giga R1 and UA4C Pin Mapping](2026-02-22_giga-r1-ua4c-pin-mapping.md)** — Giga R1 works with UA4C firmware, but Fuel Output 4 is on PA7 which does not appear in the epicEFI pin dropdown.
- **[MAX9924 Adaptive Threshold Failure on Low-Tooth Wheels](2026-02-22_max9924-adaptive-threshold-low-tooth.md)** — The MAX9924/MAX9926 VR conditioner fails on low tooth-count trigger wheels due to an 85 ms watchdog that resets the adaptive threshold.
- **[Mazda NB IACV: Polarity and PWM Frequency](2026-02-22_nb-iacv-dc-frequency.md)** — NB IACV is inverted (0% duty = open); 250 Hz PWM frequency recommended to avoid audible noise.
- **[Mazda NB Immobilizer Bypass](2026-02-22_nb-immobilizer-pinout-bypass.md)** — The NB immobilizer three-way handshake can be bypassed on aftermarket ECUs by leaving the immobilizer signal wire disconnected.
- **[SD Card Hot-Swap and Deadbug Installation](2026-02-22_sd-card-hotswap-deadbug.md)** — epicEFI supports tune hot-swap via SD card at boot; can be added to boards lacking a socket by deadbugging directly to µSD card pads.
- **[Staged Injection Bug at 93% Staging](2026-02-22_staged-injection-bug-teraflash.md)** — A staged injection bug fix in the daily build was not fully effective; erratic injector outputs appeared at 93% staging.
- **[STM32 ADC Voltage Range: 3.3V Limit](2026-02-22_stm32-adc-voltage-range.md)** — STM32 GPIO is 5V tolerant but ADC is limited to 3.6V (Vdda); epicEFI hardware uses voltage dividers to scale 0–5V sensor inputs to 0–3.3V.
- **[TCC Lockup: TPS and VSS Improvement Needed](2026-02-22_tcc-lockup-tps-vss-improvement.md)** — Current TCC lock/unlock logic considers only gear and VSS; driver demand via TPS is not factored in, causing suboptimal behavior.
- **[TunerStudio Logging via Bluetooth: Data Rate Limitations](2026-02-22_ts-logging-bluetooth-data-rate.md)** — Bluetooth is too slow and lossy for full-throttle data capture; use USB with logging locked at 250–500 samples/sec.
- **[VR Conditioner MAX9926: 85 ms Watchdog and False Triggering](2026-02-22_vr-conditioner-max9926-low-tooth.md)** — The 85 ms internal watchdog resets adaptive threshold to 0V if no pulse is seen, causing false triggers on line noise at low RPM.

### 2026-02-23

- **[Direct Injection Fuel System Sensor Config](2026-02-23_di-fuel-system-sensor-config.md)** — The fuel rail sensor input for DI systems is located under the Injection section in rusEFI/epicEFI.

### 2026-02-26

- **[A340 Transmission Line Pressure PWM on uaEFI](2026-02-26_a340-line-pressure-pwm-uaefi.md)** — Controlling A340 SLT+/SLT- line pressure solenoids via uaEFI H-bridge outputs, with PWM output limitations on the Mega_Epic board.
- **[ADC MAP Sensor Voltage Ceiling on H7 Green Board](2026-02-26_adc-map-sensor-voltage-limits.md)** — The mega128/mega144 H7 Green board ADC maxes at ~4.7V; virtually all MAP sensors stay under 4.5V so this is not a practical constraint.
- **[Autotune RPM Ceiling from Rich VE Table](2026-02-26_autotune-rpm-wall-warnings.md)** — An overly rich VE table caused spark blowout, creating an effective autotune RPM ceiling of ~4800 RPM.
- **[Aux Temperature Sensor for Intercooler Fan Control](2026-02-26_aux-temp-intercooler-fan-control.md)** — Intercooler fan control via aux temp sensor requires a logic output rule; feature request to expose analog input selector directly in the fans menu.
- **[Barometric Correction Table: Safe Defaults](2026-02-26_baro-correction-table-defaults.md)** — Baro correction table defaults to all 1.0 (unity); users without a dedicated baro sensor should leave all values at 1.0.
- **[Baro Table Fuelling Configuration](2026-02-26_baro-table-fuelling-config.md)** — Incorrectly configured baro correction will apply wrong fuelling corrections; safe default is all values set to 1.
- **[BossECU 48 SD Card PR Merged](2026-02-26_bossecu48-sdcard-pr.md)** — Pull request for BossECU 48 was confirmed ready to merge, resolving a known SD card functionality issue on that board.
- **[CAN Bus Communication Requires Lua Scripting](2026-02-26_can-bus-lua-scripting.md)** — Reading, filtering, storing, or reacting to CAN bus messages in rusEFI requires writing a Lua script.
- **[CAN Bus Community Accessories and DIY Peripherals](2026-02-26_canbus-feature-community-accessories.md)** — Community members are building their own CAN bus peripheral devices, reflecting a shift toward integrated systems engineering beyond traditional tuning.
- **[Custom ECU I/O Pin Planning](2026-02-26_custom-ecu-io-pin-planning.md)** — Pin allocation discussions for custom rusEFI/epicEFI ECU designs, including sensor ground allocation and a concrete I/O channel list for full-featured builds.
- **[Deutsch DT Connector Corrosion and Replacement](2026-02-26_dt-connector-corrosion-partial-pinout.md)** — Worn or corroded 2-pin DT connector sockets that no longer retain terminals; generic pigtail replacements are the preferred workaround.
- **[EGT Sensors via CAN Bus: DIY Options](2026-02-26_egt-sensors-can-bus-diy.md)** — Options for adding more EGT channels than the ECU supports directly, using EGT-to-CAN devices or DIY MCU builds with K-type amplifiers.
- **[epicEFI and EpicTuner: Project Overview](2026-02-26_epicefi-epictuner-project-overview.md)** — epicEFI is a rusEFI fork running on most rusEFI-compatible ECUs; EpicTuner is a TunerStudio replacement supporting epic, rusEFI, and fome firmware.
- **[ESP32 Flexible PWM Timer Capabilities](2026-02-26_esp32-pwm-timer-capabilities.md)** — ESP32 LEDC peripheral supports true flexible PWM from ~2 Hz to MHz across up to 8 pins simultaneously.
- **[ETB Feed-Forward Table Stopped Working](2026-02-26_etb-feed-forward-table.md)** — A feed-forward table in the rusEFI ETB control loop stopped working correctly after a firmware or configuration change.
- **[ETB H-Bridge Troubleshooting: Blown Fuse](2026-02-26_etb-hbridge-troubleshooting.md)** — Throttle motor not moving on a uaEFI board was caused by a faulty solder joint on the carrier board's 5A fuse.
- **[ETB TPS Calibration: Swapping H-Bridge Motor Wires](2026-02-26_etb-tps-calibration-hbridge-fix.md)** — Persistent inverted TPS readings on an ETB were fixed by physically swapping the H-bridge motor control wires.
- **[ETB TPS Calibration: Diagnosing Signal Inversion](2026-02-26_etb-tps-calibration-inversion.md)** — Correct diagnosis of ETB TPS inversion requires reading the rest-position voltage with ETB disable enabled.
- **[Adjustable FPR Tuning and Base Fuel Pressure Setup](2026-02-26_fpr-tuning-fuel-pressure-settings.md)** — To set base fuel pressure, disconnect the manifold reference line and adjust the FPR screw with the pump running until the gauge reads target pressure.
- **[Toyota/Lexus Fuel Pump Computer Bypass](2026-02-26_fuel-pump-computer-bypass-options.md)** — Toyota/Lexus FPC can be bypassed by driving a 12V relay through the existing 5V PWM signal line from the standalone ECU.
- **[GM Alternator Types and Wiring](2026-02-26_gm-alternator-types-wiring.md)** — Three distinct GM alternator generations with different wiring; Gen4 LS alternators require a 128 Hz PWM signal from the ECU.
- **[Hybrid Cable+Motor Throttle with Dual TPS](2026-02-26_hybrid-cable-motor-idle-dual-tps.md)** — A hybrid throttle body configuration where a mechanical cable and DC motor both act on the throttle blade, using two separate TPS sensors.
- **[LTFT Operation and Wideband O2 Grounding](2026-02-26_ltft-operation-and-o2-grounding.md)** — LTFT accumulates corrections continuously; a critical hardware prerequisite is grounding the wideband O2 sensor to the ECU chassis ground.
- **[MBT Ignition Timing: Dyno Method](2026-02-26_mbt-ignition-timing-dyno-tuning.md)** — Maximum Brake Torque timing is found via steady-state dyno runs, advancing timing in 1-degree increments until torque peaks.
- **[Mega_Epic CAN Bus I/O Expansion Board](2026-02-26_mega-epic-canbus-board-design.md)** — The mega_epic is a CAN-connected I/O expander for rusEFI/epicEFI systems, designed as a dedicated expansion board connecting via CAN.
- **[Mega144H7 DCwerx Board: DFU Flashing](2026-02-26_mega144h7-dcwerx-dfu-flashing.md)** — Confirming correct firmware for the DCwerx H7 adapter board and resolving difficulty entering DFU mode to flash it.
- **[Mega144H7 on Speeduino: Install Notes](2026-02-26_mega144h7-speeduino-install-notes.md)** — Notes on installing the DCwerx MEGA144H7 onto Speeduino, with SPI CS pin for the onboard SD card and CAN module layout as open questions.
- **[MOSFET and Gate Driver Selection for Adapter Boards](2026-02-26_mosfet-selection-gate-driver-adapter-boards.md)** — Comparison of VNLD5090 against a low-Rds(on) 3.3V-compatible MOSFET for custom adapter boards.
- **[Online Flasher Reliability and DFU Mode Workaround](2026-02-26_online-flasher-dfu-mode.md)** — The online flasher can fail to detect/flash boards; manually forcing DFU mode before connecting USB is the reliable workaround.
- **[Proteus 8×8 New Board Design Planning](2026-02-26_proteus-8x8-board-design-planning.md)** — Plans for a generic 8×8 rusEFI board in a GM p01 case with Proteus footprint and 3× CAN bus interfaces.
- **[Proteus MCU Upgrade: F4 → F7 → H7](2026-02-26_proteus-mcu-upgrade-f4-f7-h7.md)** — The STM32F4 on Proteus boards can be upgraded to STM32F7 or STM32H7 via hot-air rework for improved performance.
- **[RADLOK Battery Connector on ECU/PDU Boards](2026-02-26_radlok-battery-connector-pcb.md)** — Identification of the RADLOK high-current PCB connector family with specific PCB-side part number and datasheet reference.
- **[rusEFI Piggyback: VR Crank and Hall Cam Signal Output](2026-02-26_rusefi-piggyback-signal-output.md)** — Whether a rusEFI ECU can output VR crank and Hall cam signals to a factory ECU for piggyback operation.
- **[SC300 2JZ-GE PNP Wiring Notes](2026-02-26_sc300-2jzge-pnp-wiring-notes.md)** — Live troubleshooting for wiring a rusEFI/epicEFI PNP adapter to a 1992 Lexus SC300; sensor architecture findings and wiring config.
- **[SD Card Tune Backup Validation](2026-02-26_sd-card-tune-backup-validation.md)** — SD card backup must survive both hard power-disconnect and soft-reboot without data loss and boot from SD on next power-up.
- **[rusEFI Simulator on WSL2: TCP Setup](2026-02-26_simulator-tcp-wsl2-setup.md)** — Instructions for running the rusEFI TCP simulator inside WSL2 and connecting to it from Windows via the WSL2 internal network address.
- **[TCC Control Configuration and Road Testing](2026-02-26_tcc-control-configuration.md)** — Extended discussion of TCC lock/unlock behavior during real-world road testing of the epicEFI Transmission Control Unit.
- **[TCU/TCC Tuning on A340 Transmission](2026-02-26_tcu-tcc-tuning-a340.md)** — Live development session testing the epicEFI TCU on an A340 automatic transmission, covering TCC solenoid wiring and PWM frequency requirements.
- **[Retarded Timing Can Damage the O2 Sensor](2026-02-26_timing-o2-sensor-damage.md)** — Excessively retarded ignition timing causes unburned fuel to reach and damage the oxygen sensor.
- **[Torque Estimation via Fuel Energy Balance](2026-02-26_torque-estimate-fuel-energy-balance.md)** — rusEFI/epicEFI implements a torque estimation method converting the chemical energy of injected fuel into estimated shaft torque.
- **[UA4C vs uaEFI for Drive-By-Wire VW 1.8T](2026-02-26_ua4c-uaefi-dbw-vw18t.md)** — For a VW 1.8T install keeping DBW, the uaEFI board is recommended over UA4C.
- **[uaEFI Cylinder Mismatch Crash and Recovery](2026-02-26_uaefi-cylinder-mismatch-crash-recovery.md)** — Configuring a mismatched cylinder count crashes the uaEFI/Proteus firmware; recovery requires STM32CubeProgrammer.
- **[uaEFI I/O Expansion Hardware Options](2026-02-26_uaefi-io-expansion-hardware.md)** — Multiple techniques for adding additional outputs to the super uaEFI / epicEFI platform.
- **[UART8 K-Line Debugging](2026-02-26_uart8-kline-debugging.md)** — Debugging a UART8-based K-Line implementation revealed a reversed hardware connection and firmware CLT override issues.
- **[VR Sensor Default Config and Toyota Negative Wiring](2026-02-26_vr-sensor-preconfig-toyota-negatives.md)** — Board-level default VR sensor settings and Toyota-specific wiring requirements for negative sensor connections.
- **[VR Sensor Sharing with OEM ECU](2026-02-26_vr-sensor-sharing-oem-ecu.md)** — When running standalone alongside an OEM ECU, VR sensors can be shared by tapping the signal rather than cutting the wire.
- **[VW VR5 Trigger Wheel and VVT Setup](2026-02-26_vr5-trigger-vvt-setup.md)** — Trigger and VVT cam sensor configuration for a VW VR5 using a 60-2 crank wheel and two Bosch cam sensors.
- **[VTEC Control Implementation in rusEFI/epicEFI](2026-02-26_vtec-control-implementation.md)** — Development of VTEC control functionality covering recommended settings and known issues.
- **[VW MK2/MK3 ABS Retrofit Wiring](2026-02-26_vw-abs-retrofit-wiring.md)** — VW MK2/MK3 ABS retrofit wiring requirements and cross-generation parts compatibility.
- **[Water/Methanol Injection: EGT-Triggered and Decel Strategies](2026-02-26_water-injection-egt-cooling.md)** — Unconventional water/methanol injection strategies including EGT-triggered injection and spraying water during deceleration.

### 2026-02-27

- **[Analog Input YAML Configuration for Custom Boards](2026-02-27_analog-input-yaml-config-compile.md)** — How to configure pin assignments for a custom epicEFI board variant using the YAML configuration format.
- **[Crank Sensor: Input Pin vs Output Confusion](2026-02-27_crank-sensor-pin-cranking-debug.md)** — epicEFI reads a crank sensor *input* on a user-configured pin; there is no "crank output" — the firmware reacts to it.
- **[STM32 F4/F7: No Flash Writes While Engine Running](2026-02-27_f4f7-flash-burn-sd-card.md)** — On F4 and F7 boards, flash erase blocks execution, so internal flash cannot be written while the engine is running.
- **[Hall Trigger Filter Trade-Off and Gap Parameter](2026-02-27_hall-trigger-filter-hard-start.md)** — A trade-off exists between enabling and disabling the trigger filter on Hall-effect setups; adjusting the trigger gap parameter is the developer-recommended path.
- **[Honda D16Z6 Cranking Timing Configuration](2026-02-27_honda-d16z6-cranking-timing.md)** — Excessive ignition advance during cranking on a D16Z6 prevented reliable starts; the fix involves proper cranking timing settings in uaefi/rusEFI.
- **[Idle Closed-Loop, Overrev RPM Limit, and Shift Tables](2026-02-27_idle-closedloop-overrev-rpm-shift.md)** — Three related tuning topics: idle closed-loop philosophy, RPM limit tables for overrev prevention, and RPM-based shift tables.
- **[Jeep 4.0 Supercharged: ECU Selection and MAP Scaling](2026-02-27_jeep-40-ecu-selection.md)** — A supercharged 1997 Jeep GC 4.0 hit resolution and MAP scaling limits in the stock ECU, prompting standalone ECU evaluation.
- **[SD Card Tune Storage and Write-ID Arbitration](2026-02-27_sd-card-tune-storage.md)** — The write-ID system arbitrates which tune copy takes effect at boot; the higher write-ID wins.
- **[STM32F407 Mega100F4 Firmware Compatibility](2026-02-27_stm32f407-mega100f4-firmware.md)** — Confirms that Mega100F4 firmware is the correct recommendation for an STMpro Speeduino board based on the STM32F407.
- **[Trigger Logger: How to Read It and Error Code 9007](2026-02-27_trigger-logger-errors-9007.md)** — How to read the trigger logger and interpret a 9007 error code on uaEFI.

### 2026-02-28

- **[5V Sensor Supply: Cam and Oil Pressure Issues After Upgrade](2026-02-28_5v-sensor-supply-cam-oil-pressure-issues.md)** — After upgrading to 5V sensor supply, neither cam sensor appeared to work and the oil pressure sensor gave erroneous readings.
- **[Ground Loop: Laptop Charging via 12V While Connected to ECU](2026-02-28_ground-loop-laptop-ecu-usb-charging.md)** — Charging a laptop from the vehicle's 12V outlet while connected to the ECU via USB creates a ground loop; use a battery or external power bank instead.
- **[Honda D16Z6 Cranking Timing and VVT Sync Error C6675](2026-02-28_honda-d16z6-timing-vvt-sync-error.md)** — Recurring C6675 warning on a D16Z6 12/24 trigger wheel is a VVT sync position conflict; fix is to flip crank or cam edge polarity and re-time.
- **[Jeep XJ: Fully Standalone on epicEFI TCU Code](2026-02-28_jeep-xj-automatic-tcu-standalone.md)** — A turbocharged 1999 Jeep XJ was converted from piggyback to fully standalone using the epicEFI TCU code, validating its support for automatic transmissions.
- **[MOSFET Flyback Diode and Pull-Down Resistor Configuration](2026-02-28_mosfet-flyback-diode-resistor-configuration.md)** — Correct protection topology for MOSFET-driven inductive loads: flyback diode across the load plus a gate pull-down resistor.
- **[Multi-Tank Fuel Measurement](2026-02-28_multi-tank-fuel-measurement.md)** — rusEFI/epicEFI supports tracking consumption across up to 3 independent fuel tanks with 9 derived readings.
- **[NB Miata epicEFI Install: Trigger Signal Troubleshooting](2026-02-28_nb-miata-epicEFI-trigger-troubleshooting.md)** — Noisy trigger signals (particularly cam sensor) on a 2002 NB2 Miata caused no RPM reading; session produced a concrete diagnostic procedure and NB-specific wiring tips.
- **[NTC Thermistor Pull-Up Resistor Requirement](2026-02-28_ntc-thermistor-pullup-resistor-requirement.md)** — All NTC thermistor temperature sensors (CLT, IAT, oil temp) require a pull-up resistor to a reference voltage; without it the ADC cannot read a meaningful voltage.
- **[Proteus ECU Output Capabilities](2026-02-28_proteus-ecu-output-capabilities.md)** — Proteus provides 16 low-side drivers, 12 ignition/GP outputs, 4 high-side outputs, and dual H-bridges; ignition outputs can gate external high-side MOSFETs.
- **[Proteus Hardware Mod: Second CAN, Second WBO2, MCU Upgrade](2026-02-28_proteus-mcu-upgrade-h423-to-h743.md)** — Hardware modification plan adding a second CAN channel, a second wideband O2 circuit, and swapping the MCU from STM32H423 to STM32H743.
- **[Reverse Engineering Factory ECU Pull-Up Resistors](2026-02-28_reverse-engineering-factory-ecu-pullup-resistors.md)** — Methods for determining OEM pull-up resistor values (e.g. CLT on a 1999 Toyota 4Runner) by tracing the factory PCB with a multimeter and Ohm's law.
- **[Torque Reduction Bug Fix and Stim Verification](2026-02-28_torque-reduction-testing-verified.md)** — An axis configuration bug in torque reduction was fixed (2026-02-28) and all torque reduction functions were verified working on a stimulator.
- **[Troubleshooting a Non-Responsive Pressure Sensor](2026-02-28_troubleshooting-non-responsive-pressure-sensor.md)** — When an analog pressure sensor shows no output despite confirmed wiring, bench-test it by applying mouth pressure and measuring voltage with a DMM.
- **[VNT Vacuum System Configuration](2026-02-28_vnt-vacuum-system-configuration.md)** — Vacuum circuit for VNT actuation: reservoir with anti-return valve in parallel with an N75-style solenoid, feeding a PWM-controlled VNT actuator.
- **[Wideband Controller Bricked by SD Firmware Flash](2026-02-28_wideband-firmware-sd-brick-recovery.md)** — Flashing WB controller firmware via SD card or the TS "2025" update path bricks the WB MCU; recovery requires ST-LINK full chip erase and re-flash.

### 2026-03-01

- **[dTPS in VEAL Auto Tune: What It Means](2026-03-01_dtps-autotune-parameter-explained.md)** — dTPS (delta TPS) is the rate of throttle change; VEAL uses it to suspend VE corrections during transients when AFR data is unreliable.
- **[Fuel Cut Code 2, MAP Sensor Failure, and Cam Trigger Diagnosis](2026-03-01_fuel-cut-code2-and-cam-trigger-diagnosis.md)** — Fuel cut code 2 ("Settings") means a required feature is disabled; here triggered by a failed MAP sensor reading ~300 kPa combined with disabled fuel/ignition features.
- **[Pressure Transducer: kPaG (Gauge) vs Absolute MAP](2026-03-01_pressure-transducer-kpag-vs-map-absolute.md)** — Industrial pressure transducers commonly output kPaG, not absolute pressure; using one on a MAP input requires a calibration offset in the ECU.
- **[Reversed 5V/Sensor Ground Wiring: Oil Pressure Not Reading](2026-03-01_reversed-5v-sensor-ground-oil-pressure-diagnosis.md)** — A reversed 5V and sensor ground crimp caused the oil pressure sensor to read zero while all other 5V sensors (CLT, IAT, TPS, PPS) continued working.
- **[VEAL Auto Tune Tips: Cell Limits, Idle Locking, Decel Tuning](2026-03-01_veal-autotune-tuning-tips.md)** — Practical tips: keep Max Cell Percentage Change low to avoid overshoot, lock idle cells to prevent overrun-to-idle AFR instability, and lean/zero ignition in the overrun region to eliminate decel pops.
- **[Wideband Sensor Reading Full Lean After Replacement](2026-03-01_wideband-sensor-full-lean-troubleshooting.md)** — A WBO2 sensor reading permanently full lean is usually caused by powering it before engine warmup (condensation poisoning) or loose wiring inside the gauge pod.
