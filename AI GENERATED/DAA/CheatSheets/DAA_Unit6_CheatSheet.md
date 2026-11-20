# DAA Unit VI: NP-Completeness, ILP & Approximations — Exam Cheat Sheet

**Course**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Scope**: P, NP, NP-Hard, NP-Complete, Reductions, Cook-Levin, ILP, Halting Problem, Approximation (Vertex Cover, TSP), Randomized (Las Vegas, Monte Carlo, Karger's)

---

### 1. Fundamental Complexity Classes
- **P**: Decision problems **solvable** in deterministic polynomial time `O(n^c)`.
- **NP**: Decision problems **verifiable** in deterministic polynomial time given certificate `y`.
- **NP-Hard**: Problem `H` such that every problem `L ∈ NP` polynomial-time reduces to `H` (`L ≤_p H`). Can be outside NP.
- **NP-Complete**: Both `L ∈ NP` AND `L` is NP-Hard.
- **co-NP**: Problems whose NO-instances have polynomial-time certificates.

---

### 2. Polynomial Reductions (A ≤_p B) & Proof Template
- **Definition**: Function `f` computable in `O(n^c)` such that `x ∈ A ⟺ f(x) ∈ B`.
- **Rules**:
  - If `B ∈ P` ⟹ `A ∈ P`.
  - If `A` is NP-Hard ⟹ `B` is NP-Hard.
- **NP-Completeness 4-Step Proof**:
  1. Prove `L ∈ NP` (define polynomial certificate and verifier).
  2. Select known NPC problem `L'`.
  3. Construct reduction `f: instance of L' ↦ instance of L` in poly-time.
  4. Prove `x ∈ L' ⟺ f(x) ∈ L` in both directions.

---

### 3. Classic Reductions & Equivalence Links
- **Cook-Levin Theorem (1971)**: `SAT` is NP-Complete (proven via Turing machine encoding).
- **3-SAT ≤_p Clique**: Triad of 3 vertices per clause. Connect consistent literals across different clauses. Target `k = m`.
- **Complement Graph Links**:
  - `Clique in G` ⟺ `Independent Set in G'` (Complement graph).
  - `Independent Set of size k in G` ⟺ `Vertex Cover of size |V| - k in G`.

---

### 4. Integer Linear Programming (ILP)
- **LP vs ILP**: LP is in P (continuous variables). ILP is NP-Hard (integer constraints).
- **Vertex Cover 0-1 ILP**:
  `Minimize Σ x_v` subject to `x_u + x_v ≥ 1` for all `(u, v) ∈ E`, `x_v ∈ {0, 1}`.
- **0/1 Knapsack ILP**:
  `Maximize Σ v_i · x_i` subject to `Σ w_i · x_i ≤ W`, `x_i ∈ {0, 1}`.

---

### 5. Approximation Algorithms (Ratio ρ(n) ≤ 2)
- **Vertex Cover 2-Approx (Maximal Matching)**:
  - Pick edge `(u, v)`, add both endpoints to `C`, remove incident edges.
  - Proof: `|C| = 2 · |M| ≤ 2 · |C*|` (since `|C*| ≥ |M|`). Ratio = 2.
- **Metric TSP 2-Approx (MST Doubling)**:
  - 1. Find MST `T` (`Cost(T) < Cost(OPT)`).
  - 2. Double edges and find Euler tour `W` (`Cost(W) = 2 · Cost(T)`).
  - 3. Shortcut repeated nodes using triangle inequality (`Cost(Tour) ≤ Cost(W) < 2 · Cost(OPT)`).

---

### 6. Randomized Algorithms
- **Las Vegas**: Always 100% correct, runtime is random variable (e.g., Randomized QuickSort expected `O(n log n)`).
- **Monte Carlo**: Fixed runtime, correct with high probability (e.g., Karger's Min-Cut, Miller-Rabin).
- **Karger's Min-Cut**: Random edge contraction. Single run success `P ≥ 2 / n²`.
  Repeating `(n²/2) · ln n` times reduces failure to `≤ 1/n`.
