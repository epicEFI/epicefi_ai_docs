# Building rusEFI / epicEFI Firmware on Windows Using WSL2 and Docker

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Tera (@teraflopping)*

## Summary

The recommended way to build rusEFI/epicEFI firmware on a Windows machine is via WSL2 (Ubuntu) with Docker and the official `rusefibuildcontainer`. The build container handles all toolchain dependencies. For Proteus F4 hardware the build target is `build_proteus_f4.sh`. An enum-to-string generation script may need to be run separately if the snapshot folder is not present.

## Details

### Prerequisites

- Windows 10/11 with WSL2 enabled
- Ubuntu distribution installed in WSL2
- Docker installed inside WSL2 Ubuntu: `apt-get -y install docker.io`

### Build Container Setup

The rusEFI build container (`rusefibuildcontainer`) encapsulates the entire ARM cross-compiler toolchain and build environment. You do not need to install `setup_linux_environment.sh` or similar scripts separately — those are for native Linux setups. The Docker container handles everything.

1. Clone the build container repository into WSL2.
2. Build the Docker image using the script inside that repo.
3. Adjust the repo directory mount path in the container run script to match your local checkout location.

### Firmware Repository Checkout

Inside WSL2, clone the rusEFI/epicEFI firmware repository into your chosen repo directory:

```bash
git clone <repo_url> /path/to/repo
cd /path/to/repo
git submodule update --init --recursive
```

The `--recursive` flag is required — rusEFI uses nested submodules.

### Running the Build

Start the build container and attach to it, then navigate to the firmware directory and run the platform-specific build script:

```bash
# Inside the Docker container
cd /rusefi/<your-checkout-dir>
cd firmware
./build_proteus_f4.sh
```

The `build_proteus_f4.sh` script runs the full build pipeline for the Proteus F4 ECU hardware target. The build script is expected to invoke all subsidiary steps automatically, including code generation.

### Troubleshooting Build Failures

- If the build fails, search the output for `error 1` and examine the lines around it for the actual error message.
- Do not run `setup_linux_environment.sh` inside the Docker container — it is unnecessary and will not help.
- The build container repository and the firmware repository are separate. Clone the firmware repo into the directory mounted into the container, not into the build container repo itself.

### Enum-to-String Generation (if needed)

If the build fails because the `snapshot` folder is missing or enum mapping files are absent, manually run the generation script before retrying the full build:

```bash
bash ./gen_enum_to_string.sh
```

This regenerates the enum-to-string mapping headers that some parts of the build depend on. Under normal circumstances `build_proteus_f4.sh` should call this automatically, but running it manually can unblock a partial or interrupted build state.

### Adding a Custom Switch to Disable a DTC Code

To add a firmware switch that disables a specific diagnostic code (e.g., code 10), the firmware must be modified and rebuilt. The process is:

1. Identify the code check in the firmware source.
2. Add a configuration flag or condition to suppress the check.
3. Rebuild using the container + `build_proteus_f4.sh` as above.
4. Flash the resulting binary to the ECU.

There is no UI toggle for suppressing individual DTC codes in standard builds — it requires a firmware modification.
