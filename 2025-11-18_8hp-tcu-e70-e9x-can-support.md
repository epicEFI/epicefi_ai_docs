# 8HP Transmission Support via e70 TCU and e9x CAN Protocol

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @fast335xi*

## Summary

rusEFI/epicEFI can support the ZF 8HP automatic transmission by swapping in an e70 8HP TCU and using the e9x CAN protocol. This approach provides 8HP support comparable to what MAXX ECU offers.

## Details

### 8HP Support Method

> "And technically rus/epic can support 8hp already about as much as maxx does using an e70 8hp TCU swapped into any 8hp and use the e9x can protocol" — @fast335xi

Key points:
- **Target transmission**: ZF 8HP (any variant)
- **TCU swap**: Use an e70 (BMW E70 X5/X6) 8HP TCU in place of the original TCU
- **CAN protocol**: e9x CAN protocol (BMW E9x chassis — E90/E92/E93 3-series) is used for communication between the ECU and TCU
- **Compatibility**: This method works with any 8HP transmission, not just those originally equipped with the e70 TCU
- **Feature parity**: Capability is described as comparable to MAXX ECU's 8HP implementation

### Practical Notes

This approach requires sourcing an e70 8HP TCU, which may involve hardware modifications depending on the donor and target vehicle. The e9x CAN protocol must be configured in the rusEFI/epicEFI software for proper ECU-to-TCU communication (gear requests, throttle signals, RPM, etc.).
