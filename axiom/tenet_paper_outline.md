# Tenet Technical Paper Outline

## Target Venues
- **OOPSLA** (Object-Oriented Programming, Systems, Languages)
- **PLDI** (Programming Language Design and Implementation)
- **SPLASH/Onward!** (Software Innovation Track)
- **AAAI** (if emphasizing AI verification angle)

---

## Paper Structure

### 1. Title
**Suggestion:** *"Tenet: A Domain-Specific Language for Game-Theoretic Verification"*

### 2. Abstract (~250 words)
- Problem: Modeling strategic interactions requires programming expertise
- Solution: Tenet—a DSL where game theory is first-class
- Key Innovation: Games are syntax, not data structures
- Result: Compile-time correctness guarantees + Nash solver integration

### 3. Introduction (1-2 pages)
- Motivate: Why do economists/researchers need better tools?
- Gap: Python/MATLAB libraries require "thinking like a programmer"
- Contribution: Language-level game primitives
- Roadmap: Section overview

### 4. Background (1 page)
- Game Theory basics (Normal Form, Nash Equilibrium)
- Existing tools (Gambit, Nashpy) and their limitations
- DSL design principles (Hudak, Fowler)

### 5. Language Design (2-3 pages)
- **5.1 Syntax:** `game { players ... strategies ... payoff ... }`
- **5.2 Type System:** How payoff matrices are validated at compile time
- **5.3 Semantics:** What `solve` actually does (calls Nash solver)
- **5.4 Error Handling:** Mismatched strategies = syntax error, not runtime

### 6. Implementation (1-2 pages)
- Interpreter architecture (Java-based, Lox heritage)
- Solver backend (Lemke-Howson or Support Enumeration)
- Integration with Axiom Stack (Flux/Alexitha)

### 7. Evaluation (2-3 pages)
- **7.1 Expressiveness:** Model classic games (Prisoner's Dilemma, Cournot)
- **7.2 Correctness:** Compare output to Gambit/Nashpy
- **7.3 Usability:** User study or expert review (optional)
- **7.4 Performance:** Solve time for N-player games

### 8. Case Study: AI Verification (1 page)
- How Tenet serves as a "referee" for LLM outputs
- Framing hallucinations as dominated strategies
- Integration with Alexitha

### 9. Related Work (1 page)
- Gambit, Nashpy, GAMS
- Other verification DSLs (Dafny, Lean)
- Neuro-symbolic systems

### 10. Conclusion & Future Work
- Summary of contributions
- Future: Mixed strategies, extensive form games, Bayesian games

### 11. References

---

## Figures to Include
1. Syntax diagram for `game` declaration
2. Type checking flow (payoff validation)
3. Architecture diagram (Parser → Solver → Output)
4. Comparison table: Tenet vs Gambit vs Nashpy
5. Axiom Stack integration diagram

## Code Listings
1. Prisoner's Dilemma example
2. Cournot Duopoly example
3. Invalid game (showing compile-time error)
