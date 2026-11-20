# DAA Unit VI: Introduction to Theory of NP-Completeness — Complete Exam Notes

**Course Code**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Sources Used**: `DAA_Question_Bank_Ans_Final.docx`, `DAAOA_Question_Bank.docx`, `Syllabus_CS3212.pdf`  
**Mathematical Notation**: Universal Unicode text symbols (no raw LaTeX tags).

---

## 1. Complexity Classes: P, NP, co-NP, NP-Hard, and NP-Complete

### 1.1 Decision Problems vs Optimization Problems
In computational complexity theory, problems are traditionally analyzed as **decision problems**:
- **Optimization Problem**: Seeks the best possible solution (e.g., "Find a tour of minimum total cost in graph G", or "Find a clique of maximum size").
- **Decision Problem**: Asks a YES/NO question with a threshold parameter k (e.g., "Does graph G contain a traveling salesperson tour of cost ≤ k?", or "Does G contain a clique of size ≥ k?").
- **Polynomial Equivalence**: An optimization problem can be solved in polynomial time if and only if its corresponding decision problem can be solved in polynomial time (using binary search over the range of possible objective values).

### 1.2 Deterministic vs Non-Deterministic Polynomial Time
- **Deterministic Turing Machine (DTM)**: At every step, the next state transition is uniquely determined. Executes a deterministic algorithm.
- **Non-Deterministic Turing Machine (NDTM)**: An idealized computational model that operates in two phases:
  1. **Guessing Phase**: An arbitrary candidate solution (certificate / witness `y`) of polynomial length is generated non-deterministically ("guessed" out of thin air).
  2. **Verifying Phase**: A deterministic polynomial-time verification algorithm `V(x, y)` checks whether the guessed certificate `y` proves that input `x` is a YES-instance. If any guess leads to YES, the NDTM accepts `x`.

### 1.3 Formal Definitions of Complexity Classes
1. **Class P (Polynomial Time)**:
   The set of all decision problems that can be **solved** by a deterministic algorithm in polynomial time `O(n^c)` for some constant `c`.
   - Examples: Shortest Path (Dijkstra/Bellman-Ford), Minimum Spanning Tree (Prim/Kruskal), Max Flow (Edmonds-Karp), Sorting, Matrix Multiplication, Linear Programming.

2. **Class NP (Nondeterministic Polynomial Time)**:
   The set of all decision problems for which a YES-instance can be **verified** in deterministic polynomial time given a polynomial-sized certificate `y`.
   - Alternatively: Problems solvable by a Non-Deterministic Turing Machine in polynomial time.
   - Crucial Distinction: NP does **NOT** mean "Non-Polynomial". It means "Nondeterministic Polynomial"!
   - Since any problem solvable in polynomial time can also be verified in polynomial time (the algorithm itself acts as the verifier):
     ```
     P ⊆ NP
     ```

3. **Class co-NP**:
   The set of decision problems whose NO-instances can be verified in polynomial time with a polynomial certificate (the complements of languages in NP).
   - Example: TAUTOLOGY (verifying that a boolean formula is always true for every assignment).

4. **Class NP-Hard**:
   A problem `H` (decision or optimization) is **NP-Hard** if every problem `L ∈ NP` can be reduced to `H` in polynomial time (`L ≤_p H`).
   - A problem in NP-Hard is **at least as hard as the hardest problems in NP**.
   - NP-Hard problems do NOT need to be in NP (they may not even be decidable, e.g., the Halting Problem is NP-Hard).

5. **Class NP-Complete (NPC)**:
   A decision problem `L` is **NP-Complete** if it satisfies both conditions:
   1. `L ∈ NP` (a solution can be verified in polynomial time).
   2. `L` is **NP-Hard** (every problem in NP reduces to `L` in polynomial time: for all `L' ∈ NP`, `L' ≤_p L`).
   - NP-Complete problems are the **toughest problems in NP**. If ANY single NP-Complete problem can be solved in polynomial time, then **P = NP**!

---

## 2. Polynomial-Time Reductions (A ≤_p B)

### 2.1 Formal Definition of Many-One Reduction
A language / decision problem `A` is **polynomial-time reducible** to language `B` (written `A ≤_p B`) if there exists a function `f: Σ* → Σ*` computable by a deterministic polynomial-time algorithm such that for all instances `x`:
```
x ∈ A   ⟺   f(x) ∈ B
```
That is:
- If `x` is a YES-instance of `A`, then `f(x)` is a YES-instance of `B`.
- If `x` is a NO-instance of `A`, then `f(x)` is a NO-instance of `B`.

### 2.2 Crucial Consequences of Reductions
1. **Solvability (Easy transfers from right to left)**:
   If `A ≤_p B` and `B ∈ P`, then `A ∈ P`.
   *(If B is easy, A is easy, because we transform instance x into f(x) in polynomial time and solve it with B's solver).*
2. **Hardness (Hardness transfers from left to right)**:
   If `A ≤_p B` and `A` is NP-Hard, then `B` is NP-Hard.
   *(If A is hard, B must be at least as hard; otherwise an easy algorithm for B would make hard problem A easy).*
3. **Transitivity**:
   If `A ≤_p B` and `B ≤_p C`, then `A ≤_p C`.

---

## 3. The Cook-Levin Theorem & The NP-Completeness Proof Template

### 3.1 The Cook-Levin Theorem (1971)
- **Statement**: The **Boolean Satisfiability problem (SAT)** is NP-Complete:
  `SAT ∈ NPC`.
- **Significance**: Proving the very first NP-Complete problem is uniquely difficult because one must prove that **every possible problem** in NP reduces to it (`L' ≤_p SAT` for all `L' ∈ NP`). Stephen Cook and Leonid Levin achieved this by mathematically encoding the step-by-step execution of any arbitrary Non-Deterministic Turing Machine into a massive boolean formula in Conjunctive Normal Form (CNF) of polynomial length.
- Once SAT was proven NP-Complete, Richard Karp (1972) proved 21 classic combinatorial problems were NP-Complete by reducing SAT to them.

### 3.2 The Standard 4-Step Template to Prove Problem L is NP-Complete
To prove that a new problem `L` is NP-Complete in your exam, follow these exact four steps:

1. **Step 1: Prove `L ∈ NP`**:
   - Define a concise certificate `y` (candidate solution).
   - Show that `y` has polynomial size in terms of the input size: `|y| = O(|x|^k)`.
   - Describe a deterministic verifier algorithm `V(x, y)` and show it runs in polynomial time `O(|x|^c)`.
2. **Step 2: Select a known NP-Complete Problem `L'`**:
   - Choose a known NPC problem (e.g., Circuit-SAT, SAT, 3-SAT, Vertex Cover, Clique, Subset Sum, Hamiltonian Cycle).
3. **Step 3: Construct the Polynomial Reduction `f`**:
   - Describe an algorithm that takes any arbitrary instance `x` of `L'` and transforms it into an instance `f(x)` of `L`.
   - Prove that the transformation runs in polynomial time: `Time(f) = O(n^k)`.
4. **Step 4: Prove Correctness (Equivalence in Both Directions)**:
   - **Forward Direction (YES ⇒ YES)**: Prove that if `x` is a YES-instance of `L'`, then `f(x)` is a YES-instance of `L`.
   - **Reverse Direction (NO ⇒ NO or YES ⇐ YES)**: Prove that if `f(x)` is a YES-instance of `L`, then `x` is a YES-instance of `L'`.

---

## 4. Classic NP-Completeness Reductions

### 4.1 Reduction: 3-SAT ≤_p Clique
- **3-SAT**: Given a boolean formula in 3-CNF with `m` clauses `C1 ∧ C2 ∧ ... ∧ Cm`, where each clause contains exactly 3 literals: `Ci = (l_i1 ∨ l_i2 ∨ l_i3)`. Is there a truth assignment that makes the formula TRUE?
- **Clique**: Given an undirected graph `G = (V, E)` and integer `k`, does `G` contain a complete subgraph of size `k`?

#### Construction:
1. For each clause `Ci = (l_i1 ∨ l_i2 ∨ l_i3)`, create a group (triad) of 3 vertices in `G`. Total vertices `|V| = 3m`.
2. Put an undirected edge between two vertices `u` and `v` if and only if:
   - They belong to **different clauses** (different triads).
   - They are **consistent literals** (they are NOT negations of each other, e.g., `xi` and `¬xi` are NEVER connected).
3. Set target clique size `k = m` (the number of clauses).
4. Running time: `|V| = 3m`, `|E| ≤ (3m)²/2 = O(m²)`, so reduction is polynomial time `O(m²)`.

#### Correctness Proof:
- **(⇒) If 3-SAT formula φ is satisfiable**:
  Then at least one literal in each of the `m` clauses is TRUE. Pick one TRUE literal from each clause triad.
  These `m` selected vertices belong to `m` different clauses, and no two can be contradictory (both are TRUE, so neither is negation of the other).
  By construction, every pair of these `m` vertices is connected by an edge. Thus, they form a clique of size `k = m`.
- **(⇐) If G has a clique of size k = m**:
  Since edges exist only between different triads, the clique must contain exactly one vertex from each of the `m` triads.
  Because edges only connect consistent literals, no two vertices in the clique represent conflicting literals `xi` and `¬xi`.
  Assign value TRUE to every literal represented by a vertex in the clique. Assign remaining variables arbitrarily.
  This truth assignment satisfies at least one literal in every clause, satisfying the entire 3-SAT formula!

---

### 4.2 Reduction: Clique ≤_p Independent Set ≤_p Vertex Cover
These three problems are intimately linked through the **Complement Graph G' = (V, E')** where `(u, v) ∈ E' ⟺ (u, v) ∉ E`.

#### Fundamental Relationships on Graph G = (V, E):
Let `S ⊆ V` be a subset of vertices of size `k`:
1. **Clique ⟺ Independent Set**:
   `S` is a **Clique** in `G` if and only if `S` is an **Independent Set** in the complement graph `G'`.
   *(In a clique, every pair has an edge; in its complement G', no pair has an edge).*
2. **Independent Set ⟺ Vertex Cover**:
   `S` is an **Independent Set** in `G` if and only if `V - S` is a **Vertex Cover** in `G`.
   - *Proof*:
     - If `S` is an independent set, no edge in `E` has both endpoints in `S`. Therefore, every edge in `E` must have at least one endpoint outside `S` (i.e., in `V - S`). Thus, `V - S` covers all edges, making it a valid Vertex Cover.
     - Conversely, if `V - S` is a vertex cover, every edge has at least one endpoint in `V - S`. Thus, no edge can have both endpoints in `S`. Hence, `S` is an independent set.
3. **Target Parameter Translations**:
   - Max Clique of size `k` in `G` ⟺ Max Independent Set of size `k` in `G'`.
   - Max Independent Set of size `k` in `G` ⟺ Min Vertex Cover of size `|V| - k` in `G`.

---

## 5. Integer Linear Programming (ILP)

### 5.1 Definition & Contrast with LP
- **Linear Programming (LP)**:
  Optimize a linear objective function subject to linear equality and inequality constraints, where variables are **continuous real numbers** (`x_i ∈ ℝ`).
  - Complexity: **Solvable in polynomial time** (`LP ∈ P`) via Karmarkar's interior-point algorithm or the Ellipsoid method.
- **Integer Linear Programming (ILP)**:
  Optimize a linear objective subject to linear constraints, where some or all variables are restricted to **integers** (`x_i ∈ ℤ` or `x_i ∈ {0, 1}`).
  - Complexity: **NP-Hard**! Even 0-1 ILP is NP-Complete in its decision version.

### 5.2 Formulating Combinatorial Problems as 0-1 ILP

#### 1. Vertex Cover ILP Formulation:
Given graph `G = (V, E)`. Find minimum size vertex cover:
- Binary decision variable: `x_v ∈ {0, 1}` for each `v ∈ V` (`x_v = 1` if `v` is in the cover, `0` otherwise).
- **Objective Function**:
  ```
  Minimize:  Σ x_v   (summed over all v ∈ V)
  ```
- **Constraints**:
  For every edge `(u, v) ∈ E`, at least one endpoint must be chosen:
  ```
  x_u + x_v ≥ 1      for all (u, v) ∈ E
  x_v ∈ {0, 1}       for all v ∈ V
  ```

#### 2. 0/1 Knapsack ILP Formulation:
Given `n` items with values `v_i`, weights `w_i`, and knapsack capacity `W`:
- Binary variable: `x_i ∈ {0, 1}` (`x_i = 1` if item `i` is included).
- **Objective Function**:
  ```
  Maximize:  Σ (v_i · x_i)   (for i = 1 to n)
  ```
- **Constraints**:
  ```
  Σ (w_i · x_i) ≤ W
  x_i ∈ {0, 1}              for all i = 1 to n
  ```

#### 3. Set Cover ILP Formulation:
Given universe `U = {e1, e2, ..., em}` and family of subsets `S1, S2, ..., Sk`:
- Binary variable: `x_j ∈ {0, 1}` for subset `S_j`.
- **Objective Function**:
  ```
  Minimize:  Σ x_j   (for j = 1 to k)
  ```
- **Constraints**:
  Every element `e_i ∈ U` must be covered by at least one selected subset:
  ```
  Σ x_j ≥ 1          for each element e_i (summed over all j such that e_i ∈ S_j)
  x_j ∈ {0, 1}       for all j = 1 to k
  ```

---

## 6. The Halting Problem & Undecidability

### 6.1 Statement of the Halting Problem
Given the description of an arbitrary computer program / Turing machine `M` and an input string `w`, determine whether `M` will eventually halt (terminate) on input `w` or run forever in an infinite loop.
- **Formally**: Language `H_TM = { <M, w> : M is a TM and M halts on input w }`.

### 6.2 Alan Turing's Proof by Diagonalization (1936)
**Theorem**: The Halting Problem is **undecidable** (no algorithm can exist to solve it).

*Proof Outline by Contradiction*:
1. Suppose there exists a program `Halt(P, x)` that returns `TRUE` if program `P` halts on input `x`, and `FALSE` if `P` loops forever.
2. Construct a new adversarial program `Opposite(P)`:
   ```text
   function Opposite(P):
       if Halt(P, P) == TRUE:
           while (TRUE) { }  // Loop forever!
       else:
           return TRUE       // Halt immediately!
   ```
3. Now pass `Opposite` to itself as input: What does `Opposite(Opposite)` do?
   - If `Halt(Opposite, Opposite) == TRUE`: Then `Opposite(Opposite)` enters an infinite loop (it does not halt). Contradiction!
   - If `Halt(Opposite, Opposite) == FALSE`: Then `Opposite(Opposite)` returns TRUE and halts immediately. Contradiction!
4. Both cases produce an impossible logical contradiction. Therefore, the assumed algorithm `Halt` cannot exist.
- The Halting Problem is **NP-Hard** and lies entirely **outside NP** because it cannot be decided in any amount of finite time.

---

## 7. Approximation Algorithms for NP-Hard Problems

When an optimization problem is NP-Hard, polynomial-time exact algorithms are unlikely to exist. We design **Approximation Algorithms** that run in polynomial time and return solutions guaranteed to be close to the optimum.

### 7.1 Approximation Ratio ρ(n)
An algorithm has an **approximation ratio ρ(n)** if for any input of size `n`, the cost `C` of the returned solution satisfies:
```
max( C / C*, C* / C ) ≤ ρ(n)
```
Where `C*` is the cost of the optimal solution.
- For a **minimization problem**: `C ≤ ρ(n) · C*` (where `ρ(n) ≥ 1`). A ratio of 2 means the solution cost is at most 2× the optimal cost.
- For a **maximization problem**: `C ≥ (1 / ρ(n)) · C*`.

---

### 7.2 2-Approximation for Vertex Cover (Maximal Matching)

#### The Algorithm:
```text
Algorithm Approx_Vertex_Cover(G = (V, E)):
1. C = empty set
2. E' = E
3. While E' is not empty:
       Pick an arbitrary edge (u, v) from E'
       Add BOTH u and v to C:  C = C ∪ {u, v}
       Remove from E' all edges incident to either u or v
4. Return C
```

#### Correctness and Approximation Ratio Proof:
1. **Valid Vertex Cover**: The algorithm continues until no edges remain in `E'`. Since every edge removed is incident to a vertex added to `C`, every edge in `G` has at least one endpoint in `C`.
2. **Matching Property**: Let `M` be the set of edges picked in step 3. By construction, no two edges in `M` share a vertex. Thus, `M` is a **maximal matching**.
3. **Size of C**: Since we add both endpoints of each edge in `M`:
   ```
   |C| = 2 · |M|
   ```
4. **Lower Bound on Optimal Cover C***:
   Any valid vertex cover must cover every edge in `M`. Because edges in `M` are pairwise vertex-disjoint, no single vertex can cover more than one edge of `M`.
   Therefore, any valid vertex cover (including the optimal cover `C*`) must contain at least one distinct vertex for every edge in `M`:
   ```
   |C*| ≥ |M|
   ```
5. **Upper Bound Ratio**:
   ```
   |C| = 2 · |M| ≤ 2 · |C*|
   ```
   Hence, the algorithm achieves an **approximation ratio of 2** in `O(V + E)` linear time!

---

### 7.3 2-Approximation for Metric Traveling Salesperson Problem (Metric TSP)

- **Metric TSP**: TSP where edge costs satisfy the **Triangle Inequality**:
  ```
  c(u, w) ≤ c(u, v) + c(v, w)   for all vertices u, v, w
  ```
  *(A direct flight from u to w is never longer than stopping at v).*

#### The MST Doubling Algorithm:
1. Compute a **Minimum Spanning Tree (MST)** `T` of graph `G` using Prim's or Kruskal's algorithm.
2. Double every edge of `T` to create a multigraph where every vertex has an even degree.
3. Compute an **Eulerian Tour** `W` on this multigraph (visits every edge exactly once).
4. Transform `W` into a Hamiltonian cycle `H` by listing vertices in order of their first appearance, **skipping (shortcutting)** already-visited vertices.

#### Proof that Approximation Ratio is 2:
1. **MST Lower Bound**: If we delete any one edge from the optimal TSP tour `H*`, the remaining edges form a spanning tree. Since `T` is a *minimum* spanning tree:
   ```
   Cost(T) < Cost(H*)
   ```
2. **Eulerian Tour Cost**: Since every edge of `T` is traversed twice in `W`:
   ```
   Cost(W) = 2 · Cost(T) < 2 · Cost(H*)
   ```
3. **Shortcutting Cost**: By the triangle inequality, shortcutting directly between unvisited vertices never increases total path cost:
   ```
   Cost(H) ≤ Cost(W)
   ```
4. Combining the inequalities:
   ```
   Cost(H) ≤ Cost(W) = 2 · Cost(T) < 2 · Cost(H*)
   ```
   Therefore, `Cost(H) ≤ 2 · Cost(H*)`. Approximation ratio = **2**. Time complexity: `O(E log V)`.

---

## 8. Randomized Algorithms

A **randomized algorithm** makes random choices (uses random bits) during its execution.

### 8.1 Las Vegas vs Monte Carlo Algorithms

| Feature | Las Vegas Algorithm | Monte Carlo Algorithm |
| :--- | :--- | :--- |
| **Output Correctness** | **Always 100% correct** (never errs) | Correct with high probability (may output wrong answer) |
| **Running Time** | **Random variable** (probabilistic runtime) | **Deterministic / bounded** (runs in fixed time) |
| **Failure Mode** | May run longer than expected in rare cases | May produce an incorrect answer with bounded probability `ε` |
| **Classic Example** | **Randomized QuickSort** (expected `O(n log n)`) | **Karger's Min-Cut** algorithm, **Miller-Rabin Primality** |

### 8.2 Karger's Min-Cut Algorithm (Monte Carlo)
- **Goal**: Find the global minimum cut in an undirected multigraph `G = (V, E)`.
- **Algorithm (Edge Contraction)**:
  1. While the graph has more than 2 vertices:
     - Pick an edge `e = (u, v)` uniformly at random.
     - Contract `u` and `v` into a single super-vertex (delete self-loops, keep parallel edges).
  2. The remaining edges between the final 2 super-vertices define the cut.
- **Success Probability**:
  The probability that a single run finds the minimum cut is at least:
  ```
  P(Success) ≥ 2 / (n · (n - 1)) = 2 / n²
  ```
- **Error Probability Reduction by Repetition**:
  If we repeat the algorithm independently `k = (n² / 2) · ln(n)` times and take the minimum cut found across all runs:
  ```
  P(All runs fail) ≤ (1 - 2/n²)^k = (1 - 2/n²)^((n²/2) · ln n) ≤ e^(-ln n) = 1 / n
  ```
  The probability of finding the true minimum cut increases to `≥ 1 - 1/n`!
