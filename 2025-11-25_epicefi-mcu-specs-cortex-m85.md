# epicEFI Next-Generation MCU Specifications: Cortex-M85 at 800 MHz

*Source: rusEFI Discord, 2025-11-25 | Channel: 1411925674498719775*
*Contributors: ggurov (@ggurov)*

## Summary

ggurov shared specifications for a prospective next-generation MCU being considered for epicEFI. The chip features an 800 MHz ARM Cortex-M85 core with Helium MVE (vector extensions), up to 4 MB of embedded NVM (eNVM), and 1.5 MB of RAM with ECC protection — a substantial step up from the STM32-class microcontrollers used in current rusEFI hardware.

## Details

**MCU specifications (as posted):**

```
800 MHz Cortex®-M85 core with Helium MVE
Up to 4 Mbytes of eNVM and 1.5 Mbytes of RAM with ECC protection
```

**Key attributes:**

| Attribute | Value |
|-----------|-------|
| Core | ARM Cortex-M85 |
| Clock speed | 800 MHz |
| SIMD/vector | Helium MVE (M-Profile Vector Extension) |
| Flash (eNVM) | Up to 4 MB |
| RAM | 1.5 MB |
| RAM integrity | ECC (Error Correcting Code) protection |

**Context:**
This was shared in the context of evaluating future hardware platforms for epicEFI ("Next EpicEFI goal post"), not as a description of currently shipping hardware. The Cortex-M85 with Helium MVE is notably capable of DSP and ML-style workloads (Helium adds 128-bit SIMD), which prompted discussion about potential AI/ML applications in future EFI control strategies.

**Note on current hardware:**
Current rusEFI/epicEFI production boards use STM32-family microcontrollers (Cortex-M4/M7 class, typically 168–550 MHz). The specs above represent a candidate for a future board revision, not existing production hardware.
