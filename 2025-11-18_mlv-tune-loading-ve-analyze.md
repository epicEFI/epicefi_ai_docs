# MLV Tune Loading and VE Analyze Issues

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @nishirams*

## Summary

@nishirams reported multiple issues with the MLV (MegaLogViewer) interface: the right-hand panel would not allow loading a tune or selecting an AFR target table, loaded tunes did not populate the tables, and clicking VE Analyze had no effect.

## Details

### Symptoms Reported

1. **AFR target table panel (right side of MLV screen):** The control was unresponsive — could not load a tune or select the AFR target table from that panel.

2. **Tune loading:** After attempting to load a tune, the two visible tables remained empty and the tune data did not appear.

3. **VE Analyze button:** Clicking the VE Analyze button produced no response or visible action.

### Diagnostic Suggestions

- Check for error messages displayed in the MLV console or status bar when attempting to load a tune.
- Verify that the correct tune file type/format is being selected (ensure the file matches the expected MLV format).
- Confirm the tune file is not corrupted or from an incompatible firmware version.

### Notes

This thread captured the initial problem report without a confirmed resolution. These symptoms could indicate:
- A file format mismatch between the tune and the MLV version being used
- A UI state issue (e.g., MLV may need a log file loaded before the VE Analyze and AFR table features become active)
- Possible bug or configuration requirement specific to the version of MLV or rusEFI firmware in use
