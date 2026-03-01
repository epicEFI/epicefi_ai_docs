# Compiling epicEFI / rusEFI Firmware Locally with rusefibuildcontainer
*Source: rusEFI Discord, 2025-11-22 | Channel: 1356732771325968630*
*Contributors: @ggurov (ggurov)*

## Summary

Describes how to compile epicEFI (rusEFI) firmware locally using the `rusefibuildcontainer` Docker-based build environment, enabling faster firmware iteration without depending on CI builds. Covers the toolchain stack (WSL2 + Ubuntu + Docker) and the build script output location.

## Details

### Repository

`https://github.com/ggurov/rusefibuildcontainer`

### Prerequisites

- **WSL2** with **Ubuntu** installed on Windows
- **Docker** installed inside WSL2 (or Docker Desktop with WSL2 backend)

### Build Process

Clone the build container repository and run the provided build script for the target board. For Proteus F4:

```bash
./build_proteus_f4.sh
```

Output: a `.zip` file is dropped into the `artifacts/` directory at the repository root.

ggurov described the happy path:
> "the ./build_proteus_f4.sh script drops zip into artifacts dir at the root"

### Why Build Locally

During active development sessions (e.g., iterating on trigger patterns, testing firmware patches), waiting for CI builds on `content.epicefi.com` introduces delays. Local builds allow:
- Testing personal patches before submitting PRs
- Faster iteration on trigger definitions, universal trigger offsets, etc.
- Building combined patches (e.g., a trigger fix + a universal trigger enhancement in the same binary)

ggurov noted during the N63 bringup session:
> "I strongly suggest you hack up the universal trigger, and what it does — so you have a faster iteration there with offsets/angles/ratios"
> "do get the local build going for faster iteration"

### Release Firmware (No Local Build Required)

If a PR has been merged to the main branch, official builds are available at:
`https://content.epicefi.com/firmware/`

The latest Proteus F4 binary on that URL will reflect the most recently merged changes. This is suitable for users who do not need a custom patch and simply want the current release.
