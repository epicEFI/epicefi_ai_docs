# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

This is a **public mirror** of AI-generated technical documentation extracted from the epicEFI Discord community. Files are rsynced here manually from the processing environment. There is no build system, no code, and no tests — this is a documentation-only repository.

The primary ongoing task when working in this repo is **keeping README.md up to date** as new `.md` files are added.

## File Naming Convention

```
YYYY-MM-DD_short-topic-description.md   — individual topic articles (the content)
YYYY-MM-DD_condensed.json               — raw extraction batch artifacts (skip these)
```

## Document Structure

Every article follows this template:

```markdown
# Title

*Source: epicEFI Discord, YYYY-MM-DD | Channel: <channel_id>*
*Contributors: @user1, @user2*

## Summary
## Details
## Notes
```

The `## Summary` section always contains one concise paragraph — use it as the source for README entries.

## Maintaining README.md

README.md contains a chronological index of all `.md` topic files (not `.json` files, not README.md itself, not CLAUDE.md). It is grouped by date, with one bullet per file in the format:

```
- **[Human-readable title](filename.md)** — one-sentence summary drawn from the article's ## Summary section.
```

When new files are rsynced in, read each new file's `## Summary` section and append the entries to the correct date group in README.md. If a date group doesn't exist yet, create a new `### YYYY-MM-DD` heading in chronological order.

## Subject Matter Context

The docs cover the **epicEFI / rusEFI ecosystem**:

- **epicEFI** — a fork of rusEFI; runs on most rusEFI-compatible ECUs (uaEFI, VatoTuned Honda PNP, etc.)
- **epicECU** — dedicated hardware by @joesphan; runs epicEFI
- **EpicTuner** — TunerStudio replacement by @sangawku; supports epicEFI, rusEFI, and fome
- **uaEFI / Proteus** — community ECU hardware platforms that run rusEFI or epicEFI
- **mega_epic** — CAN bus I/O expansion board for the epicEFI ecosystem
