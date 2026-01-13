# 🏗️ Implementation Plan: Scaffolding Flux & Tenet
> *Bootstrapping the Axiom Languages from Zero*

**Problem:** The `local_src` directories are empty.
**Solution:** Generate the initial C prototypes for Flux and Tenet, establishing the architectural foundations defined in `axiom_architecture.md`.

## User Review Required
> [!IMPORTANT]
> Since the source code was missing, I will generate a **new** C codebase. This will be a "Prototype v0.1".

## Proposed Changes

### 1. Flux Shell (`local_src/flux/`)
We will create a C-based Bytecode VM (clox-style) modified for Shell usage.

#### [NEW] `src/main.c`
- Entry point.
- Checks if run as script (`flux script.flx`) or REPL (`flux`).
- Implements the main REPL loop.

#### [NEW] `src/vm.c` & `src/vm.h`
- The core Virtual Machine.
- **Key Feature:** Native `Pipe` object type (`OBJ_PIPE`) for shell plumbing.

#### [NEW] `src/compiler.c`
- The Parser/Compiler.
- **Key Feature:** Overloading `|` operator to emit `OP_PIPE` instructions instead of just bitwise OR.

#### [NEW] `Makefile`
- Simple build script (`make flux`).

### 2. Tenet Scheduler (`local_src/tenet/`)

#### [NEW] `src/daemon.c`
- The `tenetd` entry point.
- Sets up a timerfd for 100ms ticks.

#### [NEW] `src/solver.c`
- The Game Theory Engine.
- **Key Feature:** Pure functional implementation (no `malloc` inside the loop).

---

## Verification Plan

### Automated Tests
1.  **Build Code:** Run `make` in `local_src/flux` and `local_src/tenet`.
2.  **Test Flux REPL:**
    ```bash
    ./flux
    > print "Hello Axiom";
    ```
3.  **Test Tenet Loop:**
    ```bash
    ./tenetd --test-mode
    # Should output "Tick..." every 100ms
    ```
