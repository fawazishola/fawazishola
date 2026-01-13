# 🌌 Axiom Ecosystem: Full Context Report
> *The Neuro-Symbolic Operating System*

I have analyzed the entire `Axiom` directory on the flash drive. Here is the State of the Union.

---

## 1. The Components at a Glance

| Component | Role | Language | Current Status | Integration Readiness |
|-----------|------|----------|----------------|-----------------------|
| **Flux** | Math Shell & Scripting | **C** (clox VM) | ✅ Working VM with Math Natives | 🟡 Needs System Calls (`exec`, `pipe`) |
| **Tenet** | OS Scheduler & Solver | **Java** | ⚠️ Logic exists, but incompatible | 🔴 Needs Rewrite to C for Kernel use |
| **Alexitha** | AI Verification Engine | **Python** | ✅ Working (`transformers`) | 🟢 Userspace Only (Too heavy for kernel) |
| **AxiomLFS** | Base OS | **Bash/LFS** | 🚧 In Progress (Ch. 5) | ⚪ The Foundation |

---

## 2. Component Deep Dive

### 🌀 Flux (The "Blood")
**Location:** `/Flux/clox`
**What it is:** A lightweight Bytecode VM (based on Crafting Interpreters) designed for math.
**Capabilities:**
*   **Math Natives:** `sin`, `cos`, `exp`, `log` (Found in `vm.c`)
*   **Linear Algebra:** `vec`, `mat`, `dot`, `cross` (Found in `vm.c`)
*   **Syntax:** Python-like indentation, special symbols (`·`, `²`).
**Missing System Hooks:**
*   Current `vm.c` has no `exec()` or `fork()`. It cannot launch programs yet.
*   **Action:** Add `sys.c` with POSIX wrappers.

### ♟️ Tenet (The "Brain")
**Location:** `/Tenet`
**What it is:** A DSL for Game Theory (Nash Equilibrium Solver).
**Capabilities:**
*   **Syntax:** `game { players ... payoff ... }` (Verified in `test_games.tenet`)
*   **Logic:** Currently implemented in Java (`src/org/axiom`).
**Critical Blocker:**
*   **Java is too heavy for an OS Scheduler.** You cannot run a JVM inside the kernel loop.
*   **Action:** Port the "Solver Logic" from Java to C (`tenetd`).

### 🧠 Alexitha (The "Oracle")
**Location:** `/Alexitha`
**What it is:** A fine-tuned Qwen 2.5 7B model.
**Capabilities:**
*   **Inference:** `scripts/inference.py` runs the model using `peft` and `bitsandbytes`.
*   **Goal:** Use Flux/Tenet to *verify* its output.
**Integration:**
*   Alexitha will remain a **Userspace Application** (like a daemon). It's too big to be the "Brain" of the OS itself, but it can assist the user.

---

## 3. The Unified Architecture Vision

The goal is to link these 4 pieces:

1.  **Boot:** Axiom LFS kernels boots.
2.  **Init:** User logs in to **Flux Shell** (`/bin/flux`).
3.  **Scheduling:** **Tenet Daemon** (`tenetd`) runs in background, optimizing CPU shares based on Game Theory.
4.  **Assistance:** User asks **Alexitha**: *"Simulate optimal cooling"*.
5.  **Execution:** Alexitha writes a **Flux Script**.
6.  **Verification:** Flux runs the script. Tenet optimizes the script's resource usage.

---

## 4. Immediate Next Steps (The Plan)

Since you explicitly said *“keep tenet as it is and only use flux to do that”*, we will focus on **Flux**.

1.  **Flux System Upgrade:**
    *   Add `exec(cmd)` to run Linux commands.
    *   Add `|` operator to pipe output.
2.  **Tenet (Java) Integration:**
    *   Since we aren't rewriting it yet, we will call the Java JAR from Flux: `exec("java -jar tenet.jar game.tnt")`.
    *   This allows us to use Tenet logic from the Flux shell, even if it's slow.

**Ready to start implementing Flux System Calls?**
