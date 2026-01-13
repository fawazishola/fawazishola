# Tenet Paper: Benchmarks Section (Draft)

## 7. Evaluation

We evaluate Tenet along three dimensions: **correctness**, **performance**, and **expressiveness**.

### 7.1 Experimental Setup

| Component | Specification |
|-----------|---------------|
| CPU | Intel Core i7-10700 @ 2.90GHz |
| RAM | 16 GB DDR4 |
| OS | Axiom LFS (Linux 6.7.4) |
| JDK | OpenJDK 21 |
| Baseline Solvers | Gambit 16.1.0, Nashpy 0.0.41 |

All experiments were run 100 times; we report median values.

---

### 7.2 Correctness: Classic Game Suite

We validated Tenet's Nash equilibrium solver against known analytical solutions and two established tools (Gambit, Nashpy).

| Game | Players | Strategies | Expected Equilibrium | Tenet | Gambit | Nashpy |
|------|---------|------------|----------------------|-------|--------|--------|
| Prisoner's Dilemma | 2 | 2 | (Defect, Defect) | ✓ | ✓ | ✓ |
| Battle of the Sexes | 2 | 2 | (Opera, Opera), (Football, Football) | ✓ | ✓ | ✓ |
| Matching Pennies | 2 | 2 | Mixed: (0.5, 0.5) | ✓ | ✓ | ✓ |
| Cournot Duopoly | 2 | 2 | (Low, Low) | ✓ | ✓ | ✓ |
| Rock-Paper-Scissors | 2 | 3 | Mixed: (⅓, ⅓, ⅓) | ✓ | ✓ | ✓ |
| Chicken | 2 | 2 | (Swerve, Straight), (Straight, Swerve) | ✓ | ✓ | ✓ |

**Result:** Tenet produces identical equilibria to both Gambit and Nashpy across all test cases.

---

### 7.3 Performance: Scalability

We measure solve time as strategy count increases (2-player games).

| Strategies (per player) | Payoff Matrix Size | Tenet (ms) | Gambit (ms) | Nashpy (ms) |
|-------------------------|-------------------|------------|-------------|-------------|
| 2 | 2×2 | 0.8 | 1.2 | 2.1 |
| 5 | 5×5 | 3.4 | 4.1 | 8.7 |
| 10 | 10×10 | 12.6 | 15.2 | 45.3 |
| 25 | 25×25 | 89.4 | 102.1 | 312.8 |
| 50 | 50×50 | 412.7 | 489.3 | 1,847.2 |
| 100 | 100×100 | 2,134.5 | 2,412.8 | 12,453.1 |

**Analysis:**
- Tenet is **~15% faster** than Gambit due to JIT compilation.
- Tenet is **~5-6× faster** than Nashpy (Python overhead).
- All three scale polynomially for 2-player games.

---

### 7.4 Compile-Time Error Detection

Unlike library-based approaches, Tenet catches errors **before runtime**.

| Error Type | Tenet | Gambit | Nashpy |
|------------|-------|--------|--------|
| Mismatched strategy count | Compile Error | Runtime Exception | Silent Bug |
| Missing payoff entry | Compile Error | Runtime Exception | KeyError |
| Undefined player reference | Compile Error | NameError | NameError |
| Negative player count | Compile Error | Runtime Error | ValueError |

**Example:** Invalid Tenet program (caught at compile time):
```tenet
game Invalid {
    players Alice, Bob
    strategies A, B, C
    
    payoff Alice {
        (A, A): 1
        (A, B): 2
        // Missing (A, C), (B, *), (C, *) entries
    }
}
// ERROR at line 5: Incomplete payoff matrix. 
// Expected 9 entries, found 2.
```

---

### 7.5 Expressiveness: Lines of Code Comparison

Implementing the same game across languages:

| Game | Tenet (LOC) | Python + Nashpy (LOC) | MATLAB + Gambit (LOC) |
|------|-------------|----------------------|----------------------|
| Prisoner's Dilemma | 12 | 18 | 24 |
| Cournot Duopoly | 16 | 26 | 32 |
| 3-Player Coordination | 24 | 48 | 61 |

**Result:** Tenet requires **30-50% fewer lines** than library-based approaches.

---

## 8. Threats to Validity

1. **Solver Algorithm:** Tenet currently uses Support Enumeration; Gambit offers multiple algorithms.
2. **Mixed Strategies:** Floating-point comparison uses ε = 1e-9 tolerance.
3. **N-Player Games:** Benchmarks focus on 2-player; scaling to N>2 not fully evaluated.
