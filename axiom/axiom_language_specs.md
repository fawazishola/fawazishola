# 📜 Axiom Language Capabilities Checklist
> *The "Must-Have" technical features for Tenet and Flux to become the Operating System.*

To transform from "Just a Language" to "System Component," Tenet and Flux need specific superpowers. Here is the verification list for each.

---

## ♟️ Tenet: The Scheduler (The Brain)
**Goal:** Replace dumb heuristics (CFS) with Game Theory.
**Role:** `tenetd` Daemon.

### 1. Zero-Allocation Game Loop (The Speed Req)
*   **Capability:** The Nash Equilibrium solver must run without allocating heap memory (no garbage collection pauses).
*   **Why:** You are the scheduler. If you pause for GC, the whole computer freezes.
*   **Spec:** `calc_equilibrium()` must return in < 100µs deterministic time.

### 2. Deep Kernel Introspection (The Eyes)
*   **Capability:** FFI (Foreign Function Interface) hooks into `/proc` and Cgroups.
*   **Why:** To solve the game, you need to know who the players are. Tenet must read CPU usage, Memory pressure, and I/O wait times instantly.
*   **Spec:** `Process.list()` must map directly to the Linux kernel task struct, not a slow high-level wrapper.

### 3. Hot-Reloadable Payoff Matrices (The Strategy)
*   **Capability:** Changing the "Game Rules" without restarting the daemon.
*   **Why:** When I launch a game, I want the strategy to switch to "Low Latency" immediately. When I quit, switch back to "Throughput."
*   **Spec:** `Tenet.load_strategy("gaming.tnt")` must happen atomically at runtime.

### 4. Actuation Hooks (The Hands)
*   **Capability:** Native ability to write to `/sys/fs/cgroup` and call `renice`.
*   **Why:** Solving the game is useless if you can't enforce the specific result.
*   **Spec:** Built-in verification that checks "Did the process actually yield CPU?" after a command.

---

## ⚡ Flux: The Shell (The Blood)
**Goal:** Replace Bash. Make System Administration mathematical.
**Role:** `/bin/flux` Login Shell.

### 1. First-Class Linux Pipes (The Glue)
*   **Capability:** Operator overloading for the pipe `|` symbol.
*   **Why:** A shell *must* chain programs. `ls | grep foo` is non-negotiable.
*   **Spec:** `programA | programB` should be valid Flux syntax, where `|` creates a thread-safe buffered stream, not just a mathematical "or".

### 2. Sudo/Syscall Primitives (The Power)
*   **Capability:** Direct access to `execve`, `fork`, `kill`, and `signal`.
*   **Why:** You can't be a shell if you can't launch other programs.
*   **Spec:** `System.exec("vim")` needs to be as fast as C, handling terminal signals correctly (Ctrl+C).

### 3. Calculus on Streams (The Logic)
*   **Capability:** Apply math functions (`diff`, `avg`, `int`) to live data streams.
*   **Why:** This is the killer feature. `cat temps | diff` gives you the rate of heating.
*   **Spec:** The language needs a `Stream<float>` type that computes lazily (as data arrives), calculating derivatives in real-time.

### 4. Interactive REPL with History (The UI)
*   **Capability:** Tab-completion, history search, and syntax highlighting in the terminal.
*   **Why:** If it feels worse than Zsh, nobody will use it.
*   **Spec:** A "Line Editor" module that handles raw terminal mode (ioctl) for a smooth typing experience.

---

## 🔬 Summary of Differences

| Feature | **Flux (Shell)** | **Tenet (Scheduler)** |
|:--------|:-----------------|:-----------------------|
| **Primary Input** | User Typing / File Streams | System State (CPU/RAM) |
| **Primary Output** | Launched Processes / Math | Priority Adjustments (Nice) |
| **Timing** | Human Time (ms) | Real Time (µs) |
| **Memory** | Garbage Collection OK | **Zero GC Required** |
| **Criticality** | Component crash = Restart Shell | Component crash = **System Hang** |

### 🛑 Verification Strategy
Before we integrate these into the OS, we need to run "The Gauntlet":
1.  **Tenet Check:** Can it solve a 100-player Prisoner's Dilemma in < 1ms?
2.  **Flux Check:** Can it pipe 1GB of text through a `diff()` function without leaking memory?
