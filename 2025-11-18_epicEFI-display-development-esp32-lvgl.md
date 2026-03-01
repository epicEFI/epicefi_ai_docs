# epicEFI Display Development: ESP32/STM32S3, LVGL, TFT-ESPI, and TWAI Integration

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @BroKingscooters, @Alice, @ggurov, @Joesphan*

## Summary

An extended development discussion covering the bring-up of display hardware for epicEFI using ESP32/STM32S3 microcontrollers. Topics include using Waveshare example code as a starting point, resolving LVGL and TFT-ESPI library version conflicts, debugging serial output and reset loops, the recommended development approach (start with TWAI/CAN data acquisition first, then add display output), and the hardware plan for CAN-connected touch displays.

## Details

### Project Context and Display Hardware

@ggurov announced new menus for the epicEFI project. @Alice offered to send free display units to any contributor willing to help implement display code, noting that driver code already existed for the hardware. @Joesphan subsequently acquired a set of circular and rectangular touch displays in various sizes for development purposes.

The planned hardware interface for epicEFI-compatible displays is minimal:
- **4 wires:** power, GND, CAN-H (CANH), CAN-L (CANL)

This means the display modules communicate with the ECU purely over CAN bus, requiring no additional signal wires.

### Hardware Platforms in Use

- **BroKingscooters:** Waveshare round display with built-in ESP32-S3 (dual USB dev board variant)
- **Alice:** Separate ESP32 board with an external display

### Getting Started: Use Vendor Examples

@BroKingscooters found that the most effective starting point was the Waveshare examples for their STM32S3/S3 boards. Very little modification was needed from the vendor examples — primarily adjusting pin numbers — to get basic TFT-ESPI tests running.

> "It helps to have examples. I am using Waveshare examples for their STM32S3 board."
> "Very little had to be changed, pin numbers. At least to run simple TFT-ESPI tests."

### Library Version Issue: LVGL and TFT-ESPI

A critical issue encountered was mismatched library versions between what the project examples expected and what was installed locally. The resolution was:

1. **Delete** existing local copies of the `lvgl` and `tftespi` libraries and their configuration files
2. **Copy in** the library folders directly from the project repository
3. This ensures the exact same library versions used in the working examples are present

> "I think what got me going was deleting my lvgl and tftespi libraries and configs and just copying in the folders from the repo I used. So I was sure I had same library versions that was being called in the example."

@Alice noted that the IDE should warn about missing libraries, but version mismatches may not always produce a clear warning. She confirmed keeping her libraries intact also worked once compilation succeeded.

### Debugging: Serial Output and Continuous Reset Loop

After successfully flashing the ESP32, @BroKingscooters encountered a symptom where LVGL demos displayed only colored static and the serial monitor showed continuous resets:

> "LVGL demos just flash colored static and serial shows continual resets."

@Alice confirmed she was able to get serial output after flashing, but also encountered issues when attempting to add debugging code.

The issue with brute-forcing LVGL configuration was identified as the large number of parameters that must be set correctly — getting even one wrong can cause the reset loop.

> "My issue in this brute force on the LVGL is the sheer number of parameters to set."
> @ggurov: "Brute force is better teacher actually."

### Recommended Development Sequence

@ggurov suggested an alternative development path to avoid getting stuck on display configuration:

1. **Start with TWAI (CAN bus) first** — get the CAN interface working and receiving data from the ECU
2. **Get basic data on screen** — display simple values once CAN data is flowing, so the work is not wasted even if the full display UI is not yet complete

> "Or start from the other side, get the TWAI shit working, and grabbing data. Get super basic stuff up on the screen, so it's not a waste."

This approach decouples CAN communication validation from display driver complexity, allowing incremental progress.

### Summary of Lessons Learned

| Issue | Resolution |
|-------|-----------|
| Display not initializing | Copy library folders directly from repo (lvgl, tftespi) to ensure version match |
| Continuous resets with LVGL demos | Too many LVGL parameters misconfigured; use working Waveshare examples as baseline |
| Getting started from scratch | Use vendor (Waveshare) board examples, modify only pin numbers |
| Avoiding display deadlock | Start with TWAI/CAN data first, add display output incrementally |
