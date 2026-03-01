# epicEFI Display Compatibility: MIPI vs HDMI

*Source: rusEFI Discord, 2026-02-21 | Channel: 1474618405809295370*
*Contributors: @Joesphan*

## Summary

When connecting a display to epicEFI hardware, HDMI displays work out of the box with the stock image release. MIPI displays require additional driver support that may not be present in the base image. The epicEFI team is willing to add display drivers on request for specific MIPI panels.

## Details

### HDMI Displays

HDMI displays are supported without any image modification. They should work immediately with the official epicEFI image releases published at `https://content.epicefi.com/` (full URL truncated in source). Hardware requirements and instructions are maintained in the GitHub README on the main page.

### MIPI Displays

MIPI (Mobile Industry Processor Interface) displays are not guaranteed to work with the default image. Adding support for a specific MIPI display may require modifying the image to include the appropriate display driver. Users who find their MIPI display is unsupported should reach out to @Joesphan, who can add the driver for the specific panel.

### Image Releases

Official epicEFI image releases are published at the epicEFI content host. The GitHub main page (README) contains the setup instructions and hardware requirements for each release.
