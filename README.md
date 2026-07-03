# TC BLE Lite SDK Builder

Headless build script for the TC BLE Lite SDK using TelinkIoTStudio on Linux (or Windows).

## Prerequisites

- **TelinkIoTStudio** IDE installed (default: `/home/test/Telink/TelinkIoTStudio`)
- **TC32 toolchain** in PATH (e.g. `/home/test/program_file/Telink_Studio/tc32/bin`)
- Python 3 (for flash address normalization)

## Quick Start

```bash
./robin_build_variant.sh --sdk-root /path/to/tc_ble_lite_sdk
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
# Build with defaults (rx target, txrx_selfie ROM, flash eFuse)
./robin_build_variant.sh --sdk-root /home/test/code/tc_ble_lite_sdk-1.2.2_allinone

# Build TX target with real eFuse
./robin_build_variant.sh --sdk-root /path/to/sdk --target tx --efuse-source real

# Use a custom IDE location via environment variable
TELINK_IDE_EXE=/opt/TelinkIoTStudio ./robin_build_variant.sh --sdk-root /path/to/sdk
```

## Output

Build artifacts are placed under `flash_on_rom_lib/` inside the SDK root:

| File | Description |
|------|-------------|
| `flash_on_rom_lib.elf` | Linked ELF binary |
| `flash_on_rom_lib.bin` | Flash image (with CRC) |
| `robin_ble-nocrc.bin` | Raw binary (no CRC) |
| `flash_on_rom_lib.lst` | Extended listing |

## How It Works

1. Generates `telink_ble/vendor/blueLight/robin_build_config.h` with the requested macros
2. Invokes TelinkIoTStudio in headless mode to run the `flash_on_rom_lib` Eclipse build configuration
3. Prints a summary of build status and artifact sizes

The script is designed to work with any TC BLE Lite SDK checkout — just point `--sdk-root` at the right directory.