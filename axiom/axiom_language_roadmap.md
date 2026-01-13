# 🛣️ Axiom Languages: The Road to OS Native
> *Turning Flux and Tenet into the System Kernel Components*

**Goal:** Evolve the current language prototypes into system-critical capabilities.

---

## 📅 Architecture Overview

| Component | Current State | Target State | Critical Gap |
|-----------|---------------|--------------|--------------|
| **Flux** | Math Scripting | **Native Shell** (`/bin/flux`) | System Calls, Pipes, Interactive REPL |
| **Tenet** | Game Solver | **System Scheduler** (`tenetd`) | Zero-GC Loop, Kernel Hooks (Cgroups) |

---

## 🟢 Phase 1: Flux Shell (The "Blood") implementation
*Objective: Make Flux usable as a login shell.*

### Milestone 1.1: The System Interface
> *Flux needs to talk to Linux.*
- [ ] Implement `System.exec(cmd: String)` (fork/exec wrapper)
- [ ] Implement `System.pipe(a: Process, b: Process)`
- [ ] Add `System.env` access (PATH, HOME)

### Milestone 1.2: The Interactive REPL
> *Make it feel like a real terminal.*
- [ ] Implement Line Editor (history, arrow keys)
- [ ] Add Tab Completion for file paths
- [ ] Add Signal Handling (Ctrl+C shouldn't kill the shell)

### Milestone 1.3: Stream Calculus
> *The "Killer Feature" — Math on live data.*
- [ ] Implement `Stream` type (Lazy evaluation)
- [ ] Implement `diff(s: Stream)` and `integrate(s: Stream)`
- [ ] **Verification:** Write a script that controls fan speed based on `diff(cpu_temp)`.

---

## 🔵 Phase 2: Tenet Scheduler (The "Brain") implementation
*Objective: Make Tenet capable of managing system resources.*

### Milestone 2.1: The Zero-Allocation Solver
> *Speed is everything.*
- [ ] Profile current Nash Equilibrium solver for memory allocs.
- [ ] refactor to use Pre-allocated Arena Memory.
- [ ] **Verification:** Solve 100-player game in < 100µs.

### Milestone 2.2: The Kernel Eye (Sensors)
> *Tenet needs to see the board.*
- [ ] Implement `/proc` parser (Read CPU/RAM usage)
- [ ] Implement `/sys/fs/cgroup` interface (Read limits)

### Milestone 2.3: The Kernel Hand (Actuators)
> *Tenet needs to move the pieces.*
- [ ] Implement `renice` syscall wrapper
- [ ] Implement `cgroup.procs.write` (Move PIDs between groups)
- [ ] **Verification:** Create a daemon that automatically throttles a "compiler" process when a "video" process starts.

---

## 🔴 Phase 3: Integration (The "Body")
*Objective: Boot the OS with these components.*

- [ ] Add `/bin/flux` to `/etc/shells`
- [ ] Create `tenetd.service` systemd unit
- [ ] **Final Boss:** Boot Axiom into a Flux shell managed by the Tenet scheduler.

---

*"We don't just use the computer. We solve it."*
