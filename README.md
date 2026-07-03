# telink_builder

Headless build scripts for Telink SDK projects using TelinkIoTStudio on Linux and Windows.

## Prerequisites

- **TelinkIoTStudio** IDE installed
- **TC32 toolchain** in PATH
- Linux: Python 3
- Windows: PowerShell 5+

## Scripts

| Script | Platform | Description |
|--------|----------|-------------|
| `robin_build_variant.sh` | Linux | Bash script for TC BLE Lite SDK |
| `robin_build_variant.ps1` | Windows | PowerShell script for TC BLE Lite SDK |

## Quick Start

### Linux

```bash
./robin_build_variant.sh --sdk-root /path/to/sdk
```

### Windows

```powershell
.\robin_build_variant.ps1 -SdkRoot D:\work\tc_ble_lite_sdk
```

## Usage

### Linux (`robin_build_variant.sh`)

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

### Windows (`robin_build_variant.ps1`)

```
.\robin_build_variant.ps1 -SdkRoot PATH [options]

REQUIRED:
  -SdkRoot PATH          Root directory of the SDK (contains .project/.cproject)

OPTIONS:
  -Target tx|rx|at|selfie            Firmware target (default: rx)
  -RomType txrx|txrx_selfie|allinone ROM type (default: txrx_selfie)
  -EfuseSource flash|real|default    eFuse source (default: default)
  -FlashAddr 0x76000                 Flash mirror address (default: 0x76000)
  -FixedPins 0|1                     Fixed pins (default: 1)
  -BuildTarget NAME                  Eclipse build target (default: Robin/flash_on_rom_lib)
  -IdeExe PATH                       TelinkIoTStudio.exe (env: TELINK_IDE_EXE, fallback: E:\TelinkIoTStudio\TelinkIoTStudio.exe)
  -WorkspaceDir PATH                 Eclipse workspace directory (default: ..\telink_IDE)
  -OutputDir PATH                    Output directory for renamed artifacts (default: build_variants)
  -OutputName NAME                   Output base name (default: auto-generated)
```

## Examples

```bash
# Linux
./robin_build_variant.sh --sdk-root /home/test/code/tc_ble_lite_sdk-1.2.2_allinone
./robin_build_variant.sh --sdk-root /path/to/sdk --target tx --efuse-source real
TELINK_IDE_EXE=/opt/TelinkIoTStudio ./robin_build_variant.sh --sdk-root /path/to/sdk
```

```powershell
# Windows
.\robin_build_variant.ps1 -SdkRoot D:\work\tc_ble_lite_sdk
.\robin_build_variant.ps1 -SdkRoot D:\work\sdk -Target tx -EfuseSource real
$env:TELINK_IDE_EXE = "D:\TelinkIoTStudio\TelinkIoTStudio.exe"
.\robin_build_variant.ps1 -SdkRoot D:\work\sdk
```

## How It Works

Scripts in this repo invoke TelinkIoTStudio in headless mode to run Eclipse CDT managed build configurations. Each script:

1. Generates any required config headers in the SDK
2. Calls the IDE with `-application org.eclipse.cdt.managedbuilder.core.headlessbuild`
3. Prints a summary of build status and artifact sizes

All scripts use `--sdk-root` / `-SdkRoot` to point at the target SDK checkout, so they work across different projects and repositories.