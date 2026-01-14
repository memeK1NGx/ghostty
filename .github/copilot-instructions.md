# Ghostty AI Coding Instructions

You are an expert software engineer assisting with the Ghostty project, a fast, native, feature-rich terminal emulator written in Zig.

## Project Architecture

- **Core Logic (`src/`)**: Shared cross-platform Zig code.
  - `src/main.zig`: Entry point dispatch.
  - `src/App.zig`: Primary GUI application state and orchestration.
  - `src/lib_vt.zig`: Public API for the terminal emulation engine (heavily uses `src/terminal/`).
  - `src/renderer/`: GPU rendering logic.
- **Platform Abstraction (`src/apprt/`)**: Runtime specific implementations.
  - `src/apprt/gtk/`: GTK4 implementation for Linux/FreeBSD.
  - `macos/`: Native macOS implementation (consumes Zig code as a library).
- **Build System**: Managed entirely via `build.zig` and `src/build/`. C and C++ dependencies are often built via Zig.

## Development Workflows

### Build & Run
- **Debug Build**: `zig build` (Default, no optimization).
- **Release Build**: `zig build -Doptimize=ReleaseFast`.
- **Run**: `zig build run`.
- **Run with Valgrind**: `zig build run-valgrind`.

### Testing
- **Unit Tests**: `zig build test`
  - Runs standard Zig `test "name" {}` blocks found in source files.
  - Filter tests: `zig build test -Dtest-filter="filter string"`.
- **LibVT Tests**: `zig build test-lib-vt` (Focuses on terminal emulation tests).
- **Integration Tests**: See `test/` directory for shell-based scripts and conformance cases.

### Common Tasks
- **Update Translations**: `zig build update-translations`.
- **Formatting**: Run `zig fmt .` for Zig files, `prettier -w .` for others.

## Coding Conventions

- **Language Standard**: Use idiomatic Zig (latest stable or tip as defined in `build.zig.zon`).
- **Memory Management**:
  - Pass allocators explicitly to functions designated to allocate.
  - Use `defer` immediately after allocation for cleanup.
  - Prefer `std.heap.GeneralPurposeAllocator` for debug builds to catch leaks.
- **Error Handling**: Use Zig's error union types. Handle errors explicitly; avoid `catch unreachable` unless mathematically impossible.
- **Platform Specifics**: guard platform-specific code with `if (builtin.os.tag == .macos)` or similar `builtin` checks.
- **AI Usage**:
  - If generating complex logic, verify against `AGENTS.md` guidelines.
  - **Disclosure**: Ghostty requires disclosing AI assistance in PRs.

## Important Context

- **Global State**: `src/global.zig` contains process-level state. Access carefully.
- **Configuration**: `src/config.zig` handles user configuration.
- **External Dependencies**: Managed in `build.zig.zon` and `pkg/`.
- **Reference**: See `HACKING.md` for detailed dev setup and `AGENTS.md` for AI-specific context.
