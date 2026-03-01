# Enabling Remote and Autotune Features in epICTuner

*Source: rusEFI Discord, 2026-02-20 | Channel: 1440539911059931259*
*Contributors: @ggurov, @jeybee*

## Summary

The epICTuner application supports optional features (including remote access and autotune) that are gated behind an environment variable. Features are enabled before launching the application by setting `EPICTUNER_FEATURES` in the shell environment. The autotune feature has been present in the tool for some time but is not enabled by default.

## Details

### Enabling Features via Environment Variable

To enable both the `remote` and `autotune` features, set the `EPICTUNER_FEATURES` environment variable before running `epictuner`. On Windows:

```cmd
set EPICTUNER_FEATURES=remote,autotune
epictuner
```

Multiple features are specified as a comma-separated list in the value of `EPICTUNER_FEATURES`.

### Notes

- The autotune feature has been available in epICTuner for a while; it is not a newly added capability.
- The source code for epICTuner is not publicly available, so community knowledge of its internals is limited.
- The `remote` feature enables remote tuning capability alongside `autotune` in the above example.
