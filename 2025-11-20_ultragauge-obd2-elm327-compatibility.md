# UltraGauge EM Plus OBD2 Display Compatibility with Standard ELM327 / CAN OBD2

*Source: rusEFI Discord, 2025-11-20 | Channel: 1430309988688990298*
*Contributors: @ggurov (ggurov), @sheckta#1593 (sheckta_1593), @MarkCraftPerformance (carolinacumberland)*

## Summary

The UltraGauge EM Plus v1.4c is a compact OBD2 gauge/code reader found on AliExpress (~$7-8 USD for Gen 1 / R53 Mini fitment version). The community examined whether it would work with rusEFI/epicEFI's standard OBD2 CAN implementation. The consensus is that it should work as-is if the device uses a standard ELM327-compatible interface, since rusEFI/epicEFI responds to standard CAN-bus OBD2 requests.

## Details

### Device

**Product:** UltraGauge EM Plus v1.4c (Gen 1, originally for R53 MINI)
**Source:** AliExpress (~7.68 EUR)
**AliExpress link shared:** https://a.aliexpress.com/_Ev4cXPQ

The device is a small OBD2 display/code scanner with a hook-and-loop mount, designed to clip into the dash of an R53 MINI but usable as a generic OBD2 gauge.

Joesphan noted it appeared to be based on a standard OBD2 interface, possibly an ESP32-based design.
UltraGauge product reference: https://ultra-gauge.com/ZC/

### Compatibility Assessment

sheckta#1593 assessed: "looks to be somewhat usable if it works on a standard ELM327 or something similar."

ggurov confirmed: "standard OBD2 CAN bus will respond" and "it'll work as is, if it's standard CAN bus OBD2."

**Key requirement:** The device must use standard OBD2 CAN protocol (ISO 15765-4 CAN). If it communicates via standard ELM327-compatible OBD2 requests, no modifications to the ECU are needed — rusEFI/epicEFI handles standard OBD2 PIDs over CAN natively.

### Community OBD2 Reader Context

MarkCraftPerformance noted they had built their own OBD2 code reader and offered to assist with any specific information needs for the project.

### Automotive Ethernet Trend (Related)

fast335xi observed a broader trend in OEM automotive architecture relevant to aftermarket integration:

> "Cars coming out in the next decade [...] should be really really nice to steal displays from. They're trying to reduce how many different modules are in a car and streamline everything over Ethernet."

The move toward automotive Ethernet (e.g., 100BASE-T1, OPEN Alliance) as the backbone for in-vehicle networks is expected to simplify display/module reuse in aftermarket applications, as standardized Ethernet interfaces replace proprietary CAN-based infotainment buses.
