# Firmware Timing Architecture: 1MHz Loop, Jitter, and Hardware Compare Scheduler

*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @ggurov, @mitch0s, @joesphan*

## Summary

An extended technical discussion between epicEFI developers and contributors covered the timing architecture used for ignition and injection output scheduling. The rusEFI/epicEFI approach uses a hardware-timer-triggered 1MHz loop that checks an event queue for outputs to toggle. A developer building their own ECU firmware (Mitch) described a more precise alternative using hardware compare registers to achieve zero CPU overhead and sub-50ns output jitter, and discussed the tradeoffs between polling loops, RTOS-based approaches, and hardware-compare scheduling.

## Details

### epicEFI 1MHz Loop Architecture

- rusEFI/epicEFI runs a 1MHz loop (1 microsecond period) triggered by hardware timers
- The loop checks the event queue and determines which outputs should be on or off at that moment
- The rationale for this design: making any GPIO pin capable of serving any function without requiring dedicated hardware timer channels per output
- @ggurov: "the reason rus chose 1MHz loop is to make any pin do anything"
- The loop body is small enough to complete within 1 microsecond on the target MCU

### Jitter Characteristics

- @mitch0s asked whether epicEFI has significant jitter on ignition/injection outputs
- @ggurov confirmed the 1MHz loop approach has low jitter in practice
- However, @mitch0s noted: "The timer clock runs at 1MHz but the IRQs often have jitter depending on how the ISR handles the interrupt"
- At 1us resolution, worst-case angular error depends on engine speed — at 10,000 RPM, 1us corresponds to 0.06 degrees of crank rotation
- @mitch0s achieved 0–4us callback jitter with his EventScheduler, yielding a worst-case 0.2 degree triggering resolution at 10,000 RPM
- @joesphan noted that 1us "is a long long time" in terms of MCU clock cycles, and that reading a full register is a single clock cycle on ARM

### Scaling Concern: 16 Outputs at 1us

- @mitch0s raised a concern: with 16 outputs to check state for, a 1us polling window could become very tight
- @joesphan clarified the state check is a single register read — not iterating over individual output states — making the operation fast even for many outputs
- @mitch0s acknowledged the misunderstanding: he had assumed the loop was comparing epoch timestamps per output, not reading a hardware status register

### RTOS and Determinism

- @mitch0s stated he would not fully depend on an RTOS for timing-critical ECU code: "Lose a bit of determinism"
- The ESP32 was cited as a poor ECU candidate due to determinism concerns (non-real-time WiFi/BT stack sharing CPU)
- @mitch0s: "with well-scheduled firmware not using RTOS, it's very doable on single-core MCUs at like 250MHz"
- rusEFI runs on ChibiOS (an RTOS), but the critical timing paths use hardware timers rather than OS scheduling

### Mitch's Hybrid Hardware Compare Approach

- For his own ECU firmware, @mitch0s planned a hybrid scheduler:
  1. **Software queue**: normal priority-queue scheduling, plans the next event 5–10 microseconds ahead of time
  2. **Hardware compare**: the MCU's hardware compare register is loaded with the target timer count; hardware automatically toggles the output when the timer matches the compare register
- Result: zero CPU overhead for the actual output toggle; output fires within **50 nanoseconds** of the target epoch
- Interrupt priority structure in his EventScheduler: ignition/injection outputs and crank/cam edge capture share the highest IRQ priority; math and secondary processing run at lower priority on separate scheduler instances

### Hardware Compare Mechanism (Detail)

- The scheduler loads a compare register with the target timer value (timer running at 1MHz or faster)
- The hardware compare unit continuously checks: `timer_register == compare_register`
- When they match, the GPIO is toggled in hardware — no ISR required, no CPU cycles consumed
- The scheduler software only needs to be accurate to within 5–10us to load the compare register ahead of time; the hardware handles the final precision

### Tricore MCU Discussion

- @ggurov noted tricore (Infineon AURIX, used in Bosch OEM ECUs) would be ideal for distributing these tasks:
  - One core dedicated to trigger decoding and event tracking
  - Other cores for communications and batch processing (e.g., FFT-based knock detection, VR signal processing)
- Tricore advantages over STM32: native 0–5V I/O, native Ethernet, native CAN/LIN/FlexRay, dedicated automotive peripherals, fewer pin-function conflicts
- @joesphan: "stm32 has major disadvantages in IO compared to tricore — got a bunch of overlapping resources on the same pins"
- @mitch0s noted that on STM32, everything can still be done on a single core with proper IRQ priority assignment; tricore benefits mainly for heavy parallel workloads like comms and DSP
