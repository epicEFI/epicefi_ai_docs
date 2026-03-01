# GM SWCAN Cluster Wake-Up and Keep-Alive CAN Messages

*Source: rusEFI Discord, 2025-11-27 | Channel: 1430309988688990298*
*Contributors: SanGawku (@sangawku), Joesphan (@joesphan)*

## Summary

GM instrument clusters using GMLAN Low Speed (Single Wire CAN, SWCAN) require specific CAN messages sent by the BCM to come online. The cluster does not require a separate high-voltage wake signal to power up — Power, Ground, and SWCAN are sufficient. Two key message IDs govern power-on and keep-alive behavior. The HS_DEVICE_INFORMATION message (arbitration ID 0x100280) is broadcast by the BCM and carries device identification data.

## Details

### Bus Architecture

GM Low Speed CAN (GMLAN LS / SWCAN) uses Single Wire CAN at up to 33.3 kbit/s. All modules on the bus — PCM, BCM, instrument cluster, and others — share the same single wire through a splice component (star topology). The BCM acts as the network gateway between high-speed CAN (HSCAN, differential, 500 kbit/s) and the low-speed SWCAN network.

Signal routing for blinker indicators as an example:
```
Blinker stalk → BCM → Instrument Cluster (via SWCAN)
```
Everything instrument-cluster-related passes through SWCAN.

### Powering Up the Cluster

Minimum connections required to bring a GM cluster online standalone:
- **Power** (ignition-switched 12 V)
- **Ground**
- **SWCAN** (single wire CAN bus)

The cluster does not require a high-voltage wake signal (ISO 11898 HVWAKE) when the BCM is present. Without a BCM, the NXP MC33897 (or equivalent SWCAN transceiver chip) can generate the high-voltage wake if needed.

### Key SWCAN Message IDs (GM LS-CAN)

| Message / Arbitration ID | Description | Source |
|--------------------------|-------------|--------|
| `0x10002040` | Power command — brings the cluster online | BCM |
| `0x10028040` | Keep-alive — maintains cluster in active state | BCM |
| `0x100280` (`100280`) | `HS_DEVICE_INFORMATION` — device info broadcast | BCM |

- `0x10028040` is the keep-alive packet that must be sent periodically to prevent the cluster from sleeping.
- `0x100280` / `HS_DEVICE_INFORMATION` contains device identification information; the trailing `0x40` byte in the power and keep-alive IDs indicates the BCM as source module.

### Termination

SWCAN termination is handled dynamically by the transceiver chip (e.g., MC33897). The bus loading is dynamic — there is no fixed termination resistor in the same sense as standard CAN.

### KiCad Schematic Note

SanGawku confirmed the SWCAN transceiver circuit (using the MC33897 or equivalent) can be pinned out on a KiCad schematic with the high-voltage wake pin connected as shown in the NXP datasheet. Net labels are sufficient within a single-sheet schematic; global labels are only needed for hierarchical/multi-sheet designs.

### References

- NXP MC33897 SWCAN transceiver: https://www.nxp.com/products/MC33897
- GM Upfitter wiring diagram (2013 LD Electrical): https://www.gmupfitter.com/wp-content/uploads/2021/07/2013_LD_ElectricalPickupsChassisCabs_100713.pdf
