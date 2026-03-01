# UART8 and K-Line Debugging: Hardware Reversal, Pull-Up Resistors, and CLT Override

*Source: epicEFI Discord, 2026-02-26 | Channel: 1401895238481744052 (general-tech)*
*Contributors: @BossTheTuga, @ggurov*

## Summary

Debugging a UART8-based K-Line communication implementation revealed two independent issues: a reversed hardware connection requiring pull-up resistors, and a firmware-level CLT (coolant temperature) override point that was not producing the expected gauge response. Signal voltage anomalies on the UART8 RX pin (0–5V range despite a 3.3V pull-up) were also observed and warrant further investigation.

## Details

### UART8 Hardware Configuration

The UART8 peripheral was chosen for K-Line communication. During bring-up:

- **Hardware was reversed** — TX/RX lines were swapped; correcting polarity was required
- **Pull-up resistors needed** — K-Line (ISO 9141/KWP2000) is an open-collector bus that requires pull-up to the bus voltage. Without the pull-up, the RX signal was stuck at 0V permanently
- After adding pull-up resistors and reversing the connection, the ECU began **receiving bytes**, indicating the firmware UART configuration is likely correct

### Firmware UART8 Configuration

The firmware hall/UART configuration for UART8 was modified with assistance from GitHub Copilot AI (noted by @BossTheTuga as an unfamiliar area). If bytes are being received after the hardware fix, the firmware peripheral configuration can be considered validated.

### UART8 RX Signal Voltage Anomaly

After adding a 3.3V pull-up resistor to the UART8 RX pin, the observed signal behavior was:

- **With 3.3V pull-up**: Signal swings 0–5V (unexpected — should swing 0–3.3V with a 3.3V pull-up)
- **Without pull-up**: Signal stuck at 0V

The 0–5V swing with a 3.3V pull-up suggests either:
- The K-Line transmitter is driving the line to 5V (overriding the 3.3V pull-up), or
- There is a level mismatch between the K-Line bus voltage (12V open-collector pulled to 5V by interface circuitry) and the MCU's 3.3V I/O tolerance

This should be investigated further to confirm the UART8 RX pin is not being over-driven beyond its 3.3V absolute maximum input voltage.

### K-Line CLT Override (Firmware Debug Point)

To verify K-Line communication is correctly sending engine data to the dashboard/instrument cluster, there is a specific point in the firmware where the value sent over K-Line can be overridden:

- @ggurov identified "a spot where we override what's sent over the k-line"
- @BossTheTuga attempted to hard-code 100°C at this override point to test whether the instrument cluster gauge needle would move
- **Result**: No visible change in the gauge — the override did not produce the expected response

Next steps per @ggurov: develop diagnostic tooling to investigate what is being transmitted over the K-Line interface, to determine whether the issue is in the firmware override logic, the UART framing, or the instrument cluster's reception/decoding.

### Debugging Approach Summary

| Stage | Status | Notes |
|-------|--------|-------|
| UART8 HW polarity | Fixed | TX/RX were reversed |
| Pull-up resistor | Added | Required for open-collector K-Line bus |
| Firmware receiving bytes | Confirmed | Config likely correct |
| RX voltage with pull-up | Anomalous | 0–5V observed instead of 0–3.3V |
| CLT override test | Failed | 100°C set, gauge did not respond |
| K-Line TX diagnostic tools | Pending | @ggurov to develop |

## Notes

- K-Line (ISO 9141 / KWP2000) is a single-wire, half-duplex, open-collector protocol. The bus idles high and is driven low by the transmitter. Pull-up to the appropriate bus voltage (typically 12V via a resistor to battery, or 5V via a logic-level interface) is mandatory
- The STM32 UART8 RX pin may need a voltage divider or level-shifter if the K-Line interface is driving the line above 3.3V
- If the gauge does not respond to an override CLT value, possible causes include: incorrect K-Line message framing, wrong PID/address for CLT, or the cluster expecting checksum values that change with the overridden byte
