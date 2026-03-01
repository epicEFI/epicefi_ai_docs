# epicefi_ai_docs

AI-generated technical documentation extracted from the **epicEFI Discord** community channels.

## What This Is

This repository contains structured reference documents created by an AI (Claude) from Discord conversations in the epicEFI community. Raw support threads, troubleshooting discussions, and how-to exchanges are periodically processed and turned into clean, searchable markdown articles.

The goal is to preserve community knowledge that would otherwise be buried in Discord history and make it easy to find answers to common EFI tuning and wiring questions.

## Document Format

Each file follows the naming convention:

```
YYYY-MM-DD_short-topic-description.md
```

Documents include:
- **Summary** — a concise description of the issue or topic
- **Details** — the technical content extracted from the conversation
- **Notes** — additional context, caveats, or follow-up recommendations
- **Source metadata** — original Discord channel and contributing users

## Topics Covered

Topics span the full range of EFI system setup and tuning, including:

- Sensor wiring and troubleshooting (TPS, MAP, NTC thermistors, oil pressure)
- Trigger/crank/cam configuration and diagnostics
- ECU output capabilities and hardware configuration
- Wideband lambda sensor setup and fault recovery
- Autotune parameters (VEAL, dTPS, lambda delay)
- Engine-specific setups (Honda D-series, Mazda Miata NB, Jeep XJ, etc.)
- Ground loops, noise, and electrical interference
- Boost and VNT/VGT turbo control

## Updates

Documents are extracted and added periodically as new conversations accumulate in the Discord. Each extraction batch is dated, so you can track when content was added.
