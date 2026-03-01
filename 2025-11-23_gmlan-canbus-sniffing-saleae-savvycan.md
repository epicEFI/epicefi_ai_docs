# GMLAN CAN Bus Sniffing with Saleae Logic Analyzer and SavvyCAN

*Source: rusEFI Discord, 2025-11-23 | Channel: 1430309988688990298*
*Contributors: Joesphan (@joesphan), SanGawku (@sangawku), fast335xi (@fast335xi)*

## Summary

A team worked through the process of capturing and decoding GMLAN (GM's single-wire CAN variant) bus traffic using a Saleae logic analyzer and the SavvyCAN analysis tool. The goal was to reverse-engineer dashboard CAN messages to enable epicEFI integration with a GM dash cluster. Key technical details include the GMLAN baud rate (33.333 kbps), the Saleae export-to-SavvyCAN workflow, and the discovery that the GMLAN bus may require a 20V wake-up signal.

## Details

### GMLAN Overview

GMLAN is GM's proprietary single-wire CAN bus used primarily for body electronics (dash clusters, BCM, etc.) in early-to-mid 2000s GM vehicles. It runs at **33.333 kbps** (33,333 baud), which is significantly slower than standard CAN (500 kbps). Unlike standard CAN which uses differential pairs (CAN H / CAN L), GMLAN uses a single wire.

### Hardware Setup Attempted

**Saleae Logic Analyzer** (Logic 2 software, v2.x):
- Used to capture the GMLAN bus signal directly.
- Joesphan added a **CAN analyzer** in Logic 2 at **33,333 bps** bitrate on the CAN-H channel.
- The analyzer was able to decode frames from the captured waveform.

**CAN Transceiver Wiring Attempt (fast335xi suggestion):**
- Connect **CAN L to ground**
- Connect **GMLAN signal to CAN H**
- Baud rate: **33.3 kbps**
- Result: This wiring approach **did not work** for live capture in this instance.

**Alternative noted:** A pull-up resistor of **1 kΩ to 3.3V on CAN TX** with the transceiver in standby mode when not transmitting was suggested by fast335xi as a possible fix, but the issue was identified as the ECU's minimum CAN baud rate — the specific hardware used could not operate at 33.3 kbps.

### Saleae → SavvyCAN Export Workflow

SavvyCAN requires CAN data in a specific CSV format. Saleae Logic 2 does not export in SavvyCAN format natively. The team used the **Saleae Output Parser** script to convert:

- Tool: [https://github.com/idaholab/Saleae_Output_Parser](https://github.com/idaholab/Saleae_Output_Parser)
- The parser script requires modification to output only the three fields SavvyCAN needs: **timestamp**, **identifier**, **data**.
- The `--can` flag must be passed to the parser to avoid text formatting issues.
- Duration field and extra columns must be stripped from the output.
- All CAN IDs initially showing as `0x000` indicated a timestamp parsing issue in the script that needed to be debugged.

**Required SavvyCAN CSV format** (three columns):
```
timestamp,ID,data
```

Joesphan confirmed that once correctly formatted, the file loads into SavvyCAN for frame analysis and replay.

### GMLAN Wake-Up Signal

An important discovery from community research:
- The GMLAN bus (and the dash cluster) may require a **wake-up signal** before it will respond.
- Word from another source indicated this wake-up signal goes to approximately **20V** (though this was unconfirmed and may have been a misquote from an AI assistant).
- Reference thread for further investigation: [https://pcmhacking.net/forums/viewtopic.php?t=8205](https://pcmhacking.net/forums/viewtopic.php?t=8205)
- A simple relay circuit was suggested as a way to generate the wake-up pulse without complex electronics.

### Data Collected

Joesphan captured a Saleae `.sal` session file from a live GMLAN bus on the target vehicle. The bus showed a large volume of traffic (BCAN body messages including blinkers, wipers, headlights). SanGawku noted the importance of exercising all switches during capture to get full message coverage.

A truncated sample was also exported in SavvyCAN-compatible format (`tahoe_dash_savvycan.txt`) for initial analysis.

### Extended CAN Frame Support

The GMLAN capture showed **extended CAN frames** (29-bit identifiers), not standard 11-bit frames. The Saleae CAN analyzer must be configured accordingly.

### Goal

The end goal of this work is to enable **epicEFI PNP integration with a GM dash cluster** — driving factory gauges (RPM, coolant temp, fuel level, etc.) from the rusEFI/epicEFI ECU via GMLAN. This requires:
1. Decoding which CAN IDs and byte positions control each gauge.
2. Implementing a GMLAN transmitter in the epicEFI firmware or a gateway device.
3. Handling the GMLAN wake-up sequence.
