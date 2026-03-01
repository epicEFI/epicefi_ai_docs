# epicECU Hardware Documentation: Pinout, Wiring, DIP Switches, and Setup Guides

*Source: rusEFI Discord, 2025-11-18 | Channel: epicEFI-documentation (1376346262898475118)*
*Contributors: @joesphan*

## Summary

Several official documentation resources for the epicECU hardware were published and announced in the documentation channel. These cover physical hardware setup (DIP switches, wiring, pinout), software configuration guides (TunerStudio quick start, injector check, trigger, ETB, programmable ports), and a detailed pin description table PDF.

## Details

### Hardware Documentation Released

The following hardware documentation documents were made available:

- **DIP switch configuration** — guide for configuring the onboard DIP switches on the epicECU
- **Wiring guide** — instructions for wiring the epicECU into a vehicle
- **epicECU pinout PDF** — full connector pinout diagram

These are found under the `epicECU documentation` section of the official documentation.

### Pin Description Table

A detailed Pin Description Table PDF is available at:

**https://content.epicefi.com/documentation/pdfs/epicECU%20documentation/Pin%20Description%20Table.pdf**

This document provides the function, signal type, and notes for each pin on the epicECU connector(s).

### TunerStudio Quick Start Guide

A TunerStudio (TS) Quick Start Guide was published to help new users get up and running with the epicEFI software interface quickly.

### Additional Topics Documented

The following configuration topics were written up as standalone documentation entries:

- **Injector check** — procedure for verifying injector operation
- **Trigger** — trigger wheel configuration and setup
- **ETB (Electronic Throttle Body)** — wiring and configuration for drive-by-wire throttle
- **Programmable ports** — configuring the ECU's programmable I/O outputs

### Loading Firmware onto uaEFI

For users needing to flash firmware onto a uaEFI device, the recommended approach is to use the **rusEFI updater tool** and follow the official documentation for step-by-step instructions. An online flash tool is also available at https://content.epicefi.com/flash/ for any STM32-based ECU.
