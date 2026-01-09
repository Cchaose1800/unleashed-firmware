# Flipper Zero Unleashed Firmware - AI Coding Agent Instructions

## Project Overview
This is the Unleashed Firmware for Flipper Zero, a custom fork of the official firmware that removes regional restrictions, adds new Sub-GHz protocols, and enhances existing features while maintaining API compatibility.

**Architecture**: Embedded C/C++ firmware running on STM32WB microcontroller with FreeRTOS. Core framework is "Furi" providing OS abstractions, threading, memory management, and service registry.

**Key Directories**:
- `applications/` - User-facing apps (Sub-GHz, NFC, IR, etc.)
- `furi/` - Core framework and OS abstractions
- `lib/` - Third-party libraries (mbedTLS, FreeRTOS, nanopb, etc.)
- `scripts/` - Build and utility scripts

## Build System
- **Primary Tool**: `./fbt` (Flipper Build Tool) - SCons-based build system
- **Key Commands**:
  - `./fbt` - Build firmware (default target: `fw_dist`)
  - `./fbt updater_package` - Create update package for flashing
  - `./fbt flash` - Flash via SWD (requires debug probe)
  - `./fbt flash_usb` - Flash via USB
  - `./fbt launch_app APPSRC=path/to/app` - Build and launch specific app
- **Environment**: Toolchain auto-downloaded; no global installation needed
- **Formatting**: `./fbt format` - Auto-format code per style guide

## Application Structure
Apps are defined in `application.fam` files using Python-like syntax:

```python
App(
    appid="my_app",
    name="My Application",
    apptype=FlipperAppType.APP,
    entry_point="my_app_main",
    requires=["gui", "storage"],
    stack_size=2048,
    fap_category="Tools",
)
```

- **Types**: `APP` (main apps), `PLUGIN` (CLI extensions), `STARTUP` (boot hooks), `METAPACKAGE` (groups)
- **Dependencies**: Use `requires` for services, `provides` for offered services
- **Resources**: Icons in `icon.png`, assets in `resources/`

## Coding Conventions
- **Naming**: PascalCase for types/structs, snake_case for functions/variables
- **Files**: Prefixed (e.g., `subghz_keystore.h`), no camelCase
- **Indentation**: 4 spaces (tabs)
- **Memory**: Use Furi memory management (`malloc` → `furi_alloc`, etc.)
- **Threading**: FuriThread for concurrency, FuriMessageQueue for IPC
- **Services**: Access via FuriRecord (e.g., `furi_record_open("storage")`)

## Key Patterns
- **Service Registry**: Apps register services with `furi_record_create()`, access with `furi_record_open()`
- **Event Loops**: Use `FuriEventLoop` for async operations
- **Views**: GUI apps use ViewPort/View system from `gui` service
- **Protocol Handlers**: Sub-GHz protocols extend base classes in `lib/subghz/`
- **CLI Integration**: Apps can provide CLI commands via PLUGIN type

## Development Workflow
1. **Setup**: `git clone --recursive`, `./fbt vscode_dist` for IDE config
2. **Code**: Follow CODING_STYLE.md, use `./fbt format`
3. **Build**: `./fbt` for quick checks, `./fbt updater_package` for releases
4. **Test**: Flash with `./fbt flash_usb` or use SD card update
5. **Debug**: `./fbt debug` with GDB, or `./fbt blackmagic` for devboard

## Unleashed-Specific Features
- **Unrestricted Sub-GHz**: Regional TX limits removed, extended frequencies
- **Protocol Extensions**: Rolling codes, manual creation for FAAC/BFT/Somfy
- **External Modules**: CC1101 support via SPI
- **Bruteforce Tools**: Sub-GHz static code cracking
- **Remote Controls**: Multi-button Sub-GHz remotes

## Common Pitfalls
- **Memory Leaks**: Always pair `furi_alloc` with `free()`, use RAII-like patterns
- **Thread Safety**: GUI operations must be on main thread
- **Stack Overflow**: Check `stack_size` in manifests, default 1024 may be insufficient
- **HAL Dependencies**: Use `furi_hal_*` instead of direct STM32 registers
- **App Lifecycle**: Implement proper cleanup in exit callbacks

## Reference Files
- `CODING_STYLE.md` - Detailed style guide
- `documentation/fbt.md` - Build tool documentation
- `applications/main/subghz/application.fam` - App manifest example
- `furi/core/record.h` - Service registry API
- `lib/subghz/protocols/` - Protocol implementation examples</content>
<parameter name="filePath">c:\Users\CPB&E\Downloads\flip shit\unleashed-firmware\.github\copilot-instructions.md