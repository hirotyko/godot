# SCons Build Workflow for Godot Engine

This document describes how to build the Godot engine using SCons.

## Prerequisites

- Python 3.9+
- SCons 4.4+
- A C++ compiler (GCC 9+, Clang 6+, MSVC 2019+)
- Platform-specific dependencies (e.g., development libraries on Linux)

## Basic Build Commands

### Build the editor (default target):
```bash
scons platform=linuxbsd
```

### Build with a specific target:
```bash
scons platform=linuxbsd target=editor           # Editor (default)
scons platform=linuxbsd target=template_release  # Release export template
scons platform=linuxbsd target=template_debug    # Debug export template
```

### Specify architecture:
```bash
scons platform=linuxbsd arch=x86_64   # 64-bit (default)
scons platform=linuxbsd arch=x86_32   # 32-bit
scons platform=linuxbsd arch=arm64    # ARM 64-bit
scons platform=linuxbsd arch=arm32    # ARM 32-bit
```

## C++ Standard Selection

Override the C++ standard used for compilation:

```bash
scons platform=linuxbsd cpp_std=17    # C++17 (default)
scons platform=linuxbsd cpp_std=20    # C++20
scons platform=linuxbsd cpp_std=23    # C++23
```

This sets the appropriate compiler flag:
- GCC/Clang: `-std=gnu++XX` (with GNU extensions)
- MSVC: `/std:c++XX`

## Common Build Options

### Optimization and Debugging:
```bash
scons platform=linuxbsd target=editor dev_build=yes    # Developer build
scons platform=linuxbsd optimize=speed                  # Optimize for speed (release default)
scons platform=linuxbsd optimize=size                   # Optimize for size
scons platform=linuxbsd debug_symbols=yes               # Include debug symbols
```

### Build Features:
```bash
scons platform=linuxbsd tests=yes               # Build unit tests
scons platform=linuxbsd deprecated=no           # Disable deprecated/removed features
scons platform=linuxbsd precision=double        # Use double-precision floats
scons platform=linuxbsd threads=no              # Disable threading
scons platform=linuxbsd disable_3d=yes          # Disable 3D nodes
scons platform=linuxbsd disable_physics_3d=yes  # Disable 3D physics
```

### Compiler Settings:
```bash
scons platform=linuxbsd use_llvm=yes            # Use Clang instead of GCC (where supported)
scons platform=linuxbsd warnings=extra          # Enable extra compiler warnings
scons platform=linuxbsd werror=yes              # Treat warnings as errors
scons platform=linuxbsd verbose=yes             # Show full compiler commands
```

### Performance:
```bash
scons platform=linuxbsd num_jobs=8              # Parallel build jobs
scons platform=linuxbsd fast_unsafe=yes         # Faster incremental builds
scons platform=linuxbsd lto=thin                # Link-time optimization
scons platform=linuxbsd scu_build=yes           # Single compilation unit build

### Build Output:
```bash
scons platform=linuxbsd compiledb=yes           # Generate compile_commands.json
scons platform=linuxbsd ninja=yes               # Use Ninja backend
scons platform=linuxbsd redirect_build_objects=no  # Keep objects in source tree
```

## Custom Compiler Flags

Pass additional flags to the compiler:
```bash
scons platform=linuxbsd ccflags="-march=native" cxxflags="-fno-rtti" linkflags="-Wl,--as-needed"
```

## Available Platforms

- `linuxbsd` - Linux and BSD
- `macos` - macOS
- `windows` - Windows
- `android` - Android
- `ios` - iOS
- `web` - Web (Emscripten)

## Output Files

Built binaries are placed in the `bin/` directory with a suffix indicating build configuration:
```
bin/godot.linuxbsd.editor.x86_64       # Linux editor
bin/godot.linuxbsd.template_release.x86_64  # Linux release template
```

## Common Workflows

### Full debug build with tests:
```bash
scons platform=linuxbsd dev_build=yes tests=yes compiledb=yes
```

### Minimal release build:
```bash
scons platform=linuxbsd target=template_release optimize=speed disable_3d=yes disable_advanced_gui=yes
```

### Release build with C++23 and LTO:
```bash
scons platform=linuxbsd target=template_release optimize=speed lto=thin cpp_std=23
```
