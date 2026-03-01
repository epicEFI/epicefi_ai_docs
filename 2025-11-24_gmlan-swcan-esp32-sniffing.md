# GMLAN Low-Speed (SW-CAN) Sniffing with an ESP32S3

*Source: rusEFI Discord, 2025-11-24 | Channel: can-bus-development (1430309988688990298)*
*Contributors: SanGawku (@sangawku), Joesphan (@joesphan)*

## Summary

Hands-on session reverse-engineering a 2013 Chevrolet Tahoe (GMT900) instrument cluster over GMLAN Low-Speed (Single-Wire CAN / SW-CAN) using an ESP32S3 with a SN65HVD230DR transceiver. The session covers the non-standard 33.333 kbps baud rate, the custom CAN timing register configuration needed to achieve it, the critical discovery that Arduino CAN libraries do not support this rate (use the ESP-IDF TWAI API directly), and early packet identification results.

## Details

### GMLAN Low-Speed Protocol

- **GMLAN Low Speed = SW-CAN (Single-Wire CAN)**. These two names refer to the same protocol.
- Baud rate: **33,333 bps** (33.3 kbps). This is a non-standard rate not supported by most off-the-shelf CAN adapters or Arduino CAN libraries.
- Physical layer: single wire (CANH only; CANL is grounded internally by the transceiver in SW-CAN mode).
- CAN frame data payload is standard: **8 bytes maximum**, with the usual CAN header and flags.
- The protocol uses a mix of broadcast messages and request/response (either mode can be used; the hardware implementation determines who listens).

### Hardware Setup

| Component | Part / Detail |
|-----------|--------------|
| MCU | ESP32-S3 |
| CAN transceiver | SN65HVD230DR |
| Cluster | 2013 Chevrolet Tahoe (GMT900) |
| Cluster power | Battery voltage + ignition voltage required |
| Cluster ground | Ground connected; Low Reference pin is for steering wheel buttons, not required for cluster comms |

Wiring: battery voltage, ignition voltage, Low Speed serial data line, and ground to the cluster connector. The "Low Reference" pin on the cluster pinout is for steering wheel button inputs — it is not required for CAN communication.

### 33.333 kbps CAN Timing Configuration (ESP32 TWAI)

Standard CAN baud rate presets do not include 33.333 kbps. The following custom timing configuration achieves exactly 33.3 kbps on the ESP32:

```c
case 33333:
    t_config = {
        .brp            = 120,
        .tseg_1         = 15,
        .tseg_2         = 4,
        .sjw            = 3,
        .triple_sampling = false
    };
    break;
```

This configuration was verified to receive frames from the Tahoe cluster.

### Critical: Use TWAI API Directly, Not Arduino CAN Libraries

The SLCAN protocol (used by most USB CAN adapters and SavvyCAN) does not support non-standard baud rates such as 33.333 kbps. SavvyCAN was observed to silently fail to change the baud rate from its default (500 kbps) when 33333 was entered.

**What does work:**
- Use the ESP-IDF **TWAI** (Two-Wire Automotive Interface) API directly for custom baud rates.
- SanGawku confirmed: *"I didn't use a library. I just used the TWAI stuff. And voila."*
- Avoid higher-level Arduino CAN libraries — they abstract away the timing registers and do not expose non-standard rates.

**Alternative hardware (if no ESP32):**
- An **FT232RL** USB serial adapter supports non-standard baud rates down to 300 bps and could be used with a custom single-wire output to receive data without a full CAN controller.

### Saleae Logic Analyzer as Backup Capture Tool

The Saleae Logic analyzer can capture SW-CAN waveforms at the physical layer. Joesphan previously recorded a drive log in `.sal` format and provided a Python conversion script (`convert_saleae_to_savvycan.py`) to convert Saleae CSV output to SavvyCAN-compatible format:

```
.sal file
  → Saleae async serial analyzer export (CSV)
  → convert_saleae_to_savvycan.py
  → SavvyCAN compatible log
```

This toolchain was used to cross-reference captures between the Saleae and ESP32.

### GMT900 / 2013 Tahoe Cluster — Identified CAN Message IDs

The following message IDs were identified from the CAN log during this session:

| CAN ID (hex) | Description |
|-------------|-------------|
| `0x0C050040` | Engine RPM |
| `0x1004C040` | Fuel level |
| `0x0C030040` | Battery voltage |
| `0x1001A0C0` | Immobilizer warning light |
| *(wakeup msg)* | A wakeup message must be sent to the cluster approximately once per second; the cluster will not respond without it |

Note: the cluster also transmits its own messages onto the bus. SavvyCAN can filter by CAN ID to isolate messages sent by ECU vs. cluster.

### Gotchas and Observations

- Grounding CANL during troubleshooting will prevent the ESP32 from receiving anything; restore CANL to the transceiver before attempting RX.
- The cluster begins transmitting immediately on power-up. There is a cold-start period visible in logs.
- The GMT900 cluster's blinker state, headlight dimming level, park assist, PRNDL gear position, and TPMS warning are all transmitted over GMLAN.
- A GMLan message reference spreadsheet (community-maintained) is available at: `https://docs.google.com/spreadsheets/d/1qEwOXSr3bWoc2VUhpuIam236OOZxPc2hxpLUsV0xkn0/edit?gid=2`
