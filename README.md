# telink_builder

Headless build scripts for Telink SDK projects using TelinkIoTStudio on Linux (or Windows).

## Prerequisites

- **TelinkIoTStudio** IDE installed (default: `/home/test/Telink/TelinkIoTStudio`)
- **TC32 toolchain** in PATH (e.g. `tc32/bin` under the IDE or a standalone toolchain)
- Python 3

## Scripts

### `robin_build_variant.sh`

Build the `flash_on_rom_lib` Eclipse configuration for TC BLE Lite SDK projects.

```bash
./robin_build_variant.sh --sdk-root /path/to/sdk
```

## Usage

```
./robin_build_variant.sh --sdk-root PATH [options]

REQUIRED:
  --sdk-root PATH       Root directory of the SDK (contains .project/.cproject)

OPTIONS:
  --target tx|rx|at|selfie          Firmware target (default: rx)
  --rom-type txrx|txrx_selfie|allinone
                                    ROM type (default: txrx_selfie)
  --efuse-source flash|real|default
                                    eFuse source (default: flash)
  --flash-addr 0x76000              Flash mirror address (default: 0x76000)
  --fixed-pins 0|1                  Fixed pins (default: 1)
  --build-target NAME               Eclipse build target (default: Robin/flash_on_rom_lib)
  --ide-exe PATH                    TelinkIoTStudio executable (env: TELINK_IDE_EXE)
  --workspace PATH                  Eclipse workspace directory (default: $SDK_ROOT/../telink_IDE)
  -h, --help                        Show this help
```

## Examples

```bash
# Build with defaults
./robin_build_variant.sh --sdk-root /home/test/code/tc_ble_lite_sdk-1.2.2_allinone

# Build TX target with real eFuse
./robin_build_variant.sh --sdk-root /path/to/sdk --target tx --efuse-source real

# Use a custom IDE location
TELINK_IDE_EXE=/opt/TelinkIoTStudio ./robin_build_variant.sh --sdk-root /path/to/sdk
```

## How It Works

Scripts in this repo invoke TelinkIoTStudio in headless mode to run Eclipse CDT managed build configurations. Each script:

1. Generates any required config headers in the SDK
2. Calls the IDE with `-application org.eclipse.cdt.managedbuilder.core.headlessbuild`
3. Prints a summary of build status and artifact sizes

All scripts use `--sdk-root` to point at the target SDK checkout, so they work across different projects and repositories.