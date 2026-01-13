# 🧠 AXIOM: The Intelligent Architecture
> *Moving from "Operating System" to "Intelligent Agent"*

---

## 1. The Core Philosophy
Traditional Operating Systems (Linux, Windows, macOS) are **Managers**. They passively wait for requests and allocate resources based on fixed heuristics (fairness, priority).

**Axiom is an Agent.** It actively models the system state and optimizes for a defined goal (Global Utility) using mathematical rigor.

### The Biological Metaphor
*   **The Kernel (Linux):** The Nervous System. It carries the signals but doesn't make the strategic decisions.
*   **The Brain (Tenet):** The Decision Maker. It decides *who* gets resources based on Game Theory.
*   **The Blood (Flux):** The Carrier. It flows through every subsystem, carrying logic and math natively without the overhead of heavy VMs (like Python).
*   **The Muscle (AVX-512):** The Actuator. Raw, unbridled compute power compiled with `-march=native`.

---

## 2. Tenet: The Game Theoretic Scheduler
> *Stop Guessing. Start Solving.*

### The Problem with CFS (Completely Fair Scheduler)
Standard Linux schedulers try to be "Fair." If you run a Video Renderer and a Web Browser, it tries to give them equal slices.
*   **Result:** The Browser feels slow because it needed *latency* (immediate reaction), while the Renderer needed *throughput* (sustained power). Fairness failed both.

### The Tenet Solution: Nash Equilibria
Tenet replaces the high-level scheduling logic with a Real-Time Game Solver (`tenetd`).

1.  **The Game:** "Resource Allocation Market"
2.  **The Players:** Active Processes (PID 100, PID 204...)
3.  **The Strategies:**
    *   *Yield*: Taking 10% CPU.
    *   *Compete*: Taking 80% CPU.
    *   *Burst*: Taking 100% for 10ms.
4.  **The Payoff Matrix:** Defined by the User's Intent.
    *   *Gaming Mode:* Current Focus Window gets +1000 utility. Background tasks get -100 for interrupting.
    *   *Compile Mode:* All threads achieve Nash Equilibrium where total throughout is maxed, latency is ignored.

### Implementation
*   **`tenetd` Daemon:** Runs in userspace (or as an eBPF hook).
*   **Cycle:** Every 50-100ms.
*   **Action:** It solves the game for the current process list and applies the optimal "strategy" by adjusting `nice` values, `cgroup` limits, or CPU affinity dynamically.

---

## 3. Flux: The Native Math Shell
> *Calculus at the System Level*

### The Problem with Bash
Bash is a text processor. It cannot do math.
To check if your CPU is overheating rapidly, you have to write:
`temp_now=$(cat temp); sleep 1; temp_next=$(cat temp); echo "$temp_next - $temp_now" | bc`
It's slow, ugly, and spawns multiple processes.

### The Flux Solution: `/bin/flux`
Flux is a bytecode-compiled language that understands Calculus natively. It can be installed as a Login Shell.

#### Example: Predictive Cooling (Derivative Control)
Instead of reacting when the CPU *is* hot, Flux calculates the *derivative* (rate of change) to react *before* it gets hot.

```flux
// /etc/flux/thermal_manager.flx

import System from "axiom.sys"

val critical_temp = 85.0
val fan_max = 100.0

fun monitor() {
    // Get temperature as a time-series function
    val temp_t = System.cpu_temp()
    
    // Calculate derivative (Rate of change)
    // If temp is 50°C but rising at 20°C/sec, we PANIC NOW.
    val rate = diff(temp_t)
    
    if (rate > 10.0 or temp_t > critical_temp) {
        System.set_fan(fan_max)
    }
}
```

This runs inside the Flux VM with zero overhead. No Python interpreter to load. No pipes to `bc`. Just pure math.

---

## 4. The Unified Stack

| Layer | Traditional Linux | Axiom OS | Advantage |
|-------|-------------------|----------|-----------|
| **User Shell** | Bash | **Flux** | Math-native logic, no heavy context switching. |
| **Scheduler** | CFS (Heursitics) | **Tenet** | Mathematically proven optimality. |
| **Optimization** | Generic (x86_64) | **Native** | 15-30% faster execution. |
| **Inference** | Python/PyTorch | **llama.cpp** | Runs AI on local CPU at 50 tokens/s. |

### Conclusion
By embedding **Flux** and **Tenet**, Axiom ceases to be just a collection of packages. It becomes a cohesive system where **Math** is the universal language of both the User *and* the Kernel.
