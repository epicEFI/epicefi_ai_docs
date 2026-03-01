# Infineon Tricore Bare-Metal Bring-Up on ShieldBuddy TC275 (LED Blink)

*Source: rusEFI Discord, 2025-11-25 | Channel: epicEFI dev (1356732771325968630)*

*Contributors: ggurov (ggurov), fast335xi (fast335xi)*

## Summary

ggurov successfully brought up a bare-metal C program on the Infineon AURIX TC275 (ShieldBuddy board) running at 300 MHz, blinking the onboard LED at a 1-second period. The approach uses the AURIX iLLD (Infineon Low-Level Driver) library. A busy-delay loop calibrated to the 300 MHz CPU clock provides timing. fast335xi was also working to build the project outside Infineon's proprietary development studio (AURIX Development Studio).

## Details

### Hardware

- **Board**: ShieldBuddy TC275 (Arduino Due form-factor AURIX TC275 board)
- **CPU**: Infineon AURIX TriCore TC275, configured at **300 MHz**
- **LED pin**: Port **P14**, Pin **0** — maps to Arduino header pin **D22**

### Startup Sequence

```c
int main(void) {
    SystemInit();              // Infineon iLLD system initialisation
    IfxCpu_enableInterrupts(); // Enable CPU interrupts
    disable_watchdogs();       // Disable watchdog timers (required early in bare-metal init)
    init_led_pin();            // Configure P14.0 as GPIO output

    while (1) {
        IfxPort_togglePin(LED_PORT, LED_PIN);
        busy_delay(LED_DELAY_LOOPS);
    }
}
```

### Pin and Timing Defines

```c
#define LED_PORT        (&MODULE_P14)
#define LED_PIN         (0U)          /* ShieldBuddy D22 */
#define LED_DELAY_LOOPS (1500000u)    /* ~250 ms @ 300 MHz — adjust as needed */
```

For a 1-second blink period (toggle every 500 ms, giving 1 s full cycle):

```c
#define LED_DELAY_LOOPS (12000000u)   /* ~1 second @ 300 MHz */
```

The constant 12,000,000 comes from approximately 1 NOP per clock cycle at 300 MHz:
- 300,000,000 cycles/s ÷ (loop overhead ~25 cycles per iteration) ≈ 12,000,000 iterations/s
  - In practice, each loop iteration of the busy-delay below is treated as approximately 1 cycle for the purpose of this calibration; tuning against actual scope measurement is recommended.

### Busy-Delay Implementation

```c
static void busy_delay(uint32_t cycles) {
    for (uint32_t i = 0; i < cycles; ++i) {
        __asm__ volatile("nop");
    }
}
```

- `__asm__ volatile("nop")` prevents the compiler from optimizing the loop away.
- At 300 MHz the loop overhead means each "cycle" count corresponds to roughly 1 ns of delay, though exact timing depends on compiler optimisation and pipeline effects; use a scope to verify.

### CPU Clock Configuration

- The AURIX TC275 defaults to 20 MHz on reset; the PLL must be configured to reach 300 MHz.
- ggurov observed the chip initially running at 20 MHz before PLL configuration was applied, which caused the LED blink rate to be ~15× too slow.
- Clock configuration is handled inside `SystemInit()` / the iLLD PLL setup routines.

### Build Environment

- fast335xi was working to build the project **outside** Infineon's AURIX Development Studio (Eclipse-based).
- Recommended alternative: use the iLLD Makefiles or CMake with a TriCore GCC toolchain (`tricore-elf-gcc`).
- The ggurov build workflow used an AI coding assistant (Cursor/"vibe coding") to generate the initial boilerplate, then validated on hardware with a scope.
