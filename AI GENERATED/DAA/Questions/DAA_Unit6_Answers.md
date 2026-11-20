# DAA Unit VI: Introduction to Theory of NP-Completeness — Complete Solutions & Marking Scheme

**Course Code**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Sources Used**: `DAA_Question_Bank_Ans_Final.docx`, `DAAOA_Question_Bank.docx`, `Syllabus_CS3212.pdf`  
**Mathematical Notation**: Universal Unicode text symbols.

---

## Section A Solutions (4 Marks Each)

### Q1 Solution: Complexity Classes Definitions & Venn Diagram
**Marking Scheme**:
- Definitions of P, NP, NP-Hard, NP-Complete: 2.5 Marks (approx 0.6M each)
- Venn diagram sketch showing P ≠ NP hierarchy: 1.5 Marks

**Answer**:
1. **Class P**: The class of decision problems that can be **solved** by a deterministic Turing machine in polynomial time `O(n^c)`. Examples: Minimum Spanning Tree, Shortest Path, Max Flow.
2. **Class NP**: The class of decision problems for which a proposed candidate solution (certificate) can be **verified** in deterministic polynomial time. (Equivalently, solvable by a non-deterministic machine in polynomial time).
3. **Class NP-Hard**: A problem `H` such that every problem `L ∈ NP` can be polynomial-time reduced to `H` (`L ≤_p H`). It is at least as hard as any problem in NP and need not be in NP (e.g., Halting Problem).
4. **Class NP-Complete (NPC)**: A problem `L` that is both in **NP** and is **NP-Hard** (`L ∈ NP ∩ NP-Hard`). If any NPC problem is in P, then `P = NP`.

**Venn Diagram (Assuming P ≠ NP)**:
```text
+-------------------------------------------------------+
|                       NP-HARD                         |
|   +-------------------------------+   Halting Problem |
|   |              NP               |   Optimization TSP|
|   |   +-------+   +-----------+   |                   |
|   |   |   P   |   |NP-COMPLETE|   |                   |
|   |   | (MST, |   | (3-SAT,   |   |                   |
|   |   |  BFS) |   |  Clique,  |   |                   |
|   |   +-------+   |  VC, TSP) |   |                   |
|   |               +-----------+   |                   |
|   +-------------------------------+                   |
+-------------------------------------------------------+
```

---

### Q2 Solution: Deterministic vs Non-Deterministic Polynomial Time
**Marking Scheme**:
- Definition of deterministic polynomial time: 1.5 Marks
- Definition of non-deterministic polynomial time (guess & verify): 1.5 Marks
- Explanation of what "NP" stands for: 1 Mark

**Answer**:
1. **Deterministic Polynomial Time**:
   The computation proceeds along a single, unique sequence of states. For any given input of size `n`, the machine makes uniquely determined choices and produces the answer in time bounded by `O(n^c)`.
2. **Non-Deterministic Polynomial Time**:
   The machine can conceptualize multiple execution branches simultaneously. It operates in two stages:
   - *Guessing Stage*: A candidate solution (certificate `y` of polynomial length) is generated non-deterministically.
   - *Verification Stage*: A deterministic algorithm checks whether `y` is a valid proof that the answer is YES in polynomial time `O(n^k)`.
3. **Why NP does NOT mean "Non-Polynomial"**:
   "NP" stands for **Nondeterministic Polynomial time**. Problems in NP are indeed polynomial in nature—specifically, they are verifiable in polynomial time. Furthermore, since any problem solvable in poly-time is also verifiable in poly-time, `P ⊆ NP`.

---

### Q3 Solution: The Cook-Levin Theorem & Significance
**Marking Scheme**:
- Formal statement of the theorem: 2 Marks
- Foundational significance and reason why it was difficult: 2 Marks

**Answer**:
1. **Theorem Statement**:
   The **Boolean Satisfiability problem (SAT)** is **NP-Complete**:
   `SAT ∈ NP` and for every language `L' ∈ NP`, `L' ≤_p SAT`.
2. **Foundational Significance**:
   - Before this theorem (proven independently by Stephen Cook in 1971 and Leonid Levin in 1973), it was unknown whether any NP-Complete problem actually existed.
   - Proving the *first* NP-Complete problem is uniquely challenging because one cannot rely on a reduction from an existing NPC problem. Cook and Levin proved that the execution of *any arbitrary* Non-Deterministic Turing Machine running in polynomial time can be simulated and encoded as a boolean formula in Conjunctive Normal Form (CNF) of polynomial size.
   - Once SAT was established as NP-Complete, it served as the **master anchor**: all subsequent problems (e.g. Richard Karp's 21 classic problems) could be proven NP-Complete simply by reducing SAT (or 3-SAT) to them.

---

### Q4 Solution: Las Vegas vs Monte Carlo Randomized Algorithms
**Marking Scheme**:
- Correctness comparison: 1.5 Marks
- Running time comparison: 1.5 Marks
- Classic examples of each: 1 Mark

**Answer**:

| Comparison Criterion | Las Vegas Algorithm | Monte Carlo Algorithm |
| :--- | :--- | :--- |
| **Correctness** | **Always 100% correct**. Never produces an incorrect answer. | Produces the correct answer with **high probability**, but has a non-zero probability of error. |
| **Running Time** | **Random variable**. Expected time is polynomial, but worst-case may be high. | **Deterministic / strictly bounded**. Always finishes within a fixed time bound. |
| **Classic Example** | **Randomized QuickSort** (always sorts correctly, expected time `O(n log n)`). | **Karger's Min-Cut algorithm** (finds min cut with prob `≥ 2/n²`), **Miller-Rabin Primality Test**. |

---

## Section B Solutions (8 Marks Each)

### Q5 Solution: Polynomial Reductions & 4-Step NPC Template
**Marking Scheme**:
- (a) Definition of reduction and explanation of left/right implications: 4 Marks
- (b) 4-step proof template: 4 Marks

**Answer**:
**(a) Polynomial-Time Reduction (A ≤_p B)**:
A decision problem `A` is polynomial-time reducible to problem `B` (written `A ≤_p B`) if there exists a polynomial-time computable function `f: Σ* → Σ*` such that for every input string `x`:
```
x ∈ A   ⟺   f(x) ∈ B
```
- **Solvability transfers from right to left (`B ∈ P ⟹ A ∈ P`)**:
  If we have an efficient polynomial algorithm for `B`, we can solve any instance `x` of `A` by first converting it to `f(x)` in polynomial time, and then running the algorithm for `B`. Thus `A` is also easily solvable.
- **Hardness transfers from left to right (`A is NP-Hard ⟹ B is NP-Hard`)**:
  If `A` is already known to be computationally hard, `B` must be at least as hard as `A`. If `B` were easy, it would contradict the hardness of `A` because we could use `B` to solve `A` easily.

**(b) 4-Step Template to Prove Problem L is NP-Complete**:
1. **Step 1: Prove `L ∈ NP`**:
   State the certificate `y` (candidate solution). Show `|y|` is bounded by a polynomial in `|x|`. Describe a deterministic verifier `V(x, y)` and show it runs in polynomial time.
2. **Step 2: Select a known NP-Complete Problem `L'`**:
   Choose a known NPC problem (e.g., 3-SAT, Vertex Cover, Clique).
3. **Step 3: Construct the Polynomial-Time Reduction `f`**:
   Provide an explicit mapping algorithm that converts any arbitrary instance `x` of `L'` into an instance `f(x)` of `L`. Prove that the construction takes polynomial time `O(n^k)`.
4. **Step 4: Prove Correctness in Both Directions**:
   - *Forward*: If `x` is a YES-instance of `L'`, prove `f(x)` is a YES-instance of `L`.
   - *Reverse*: If `f(x)` is a YES-instance of `L`, prove `x` is a YES-instance of `L'`.

---

### Q6 Solution: Polynomial Equivalence of Independent Set & Vertex Cover
**Marking Scheme**:
- Formal definitions of both decision problems: 2 Marks
- Theorem statement and bidirectional proof: 4 Marks
- Polynomial equivalence and parameter conversion: 2 Marks

**Answer**:
1. **Formal Definitions**:
   - **Independent Set (IS)**: Given undirected graph `G = (V, E)` and integer `k`, does there exist a subset `S ⊆ V` of size `|S| ≥ k` such that no two vertices in `S` are joined by an edge in `E`?
   - **Vertex Cover (VC)**: Given undirected graph `G = (V, E)` and integer `k'`, does there exist a subset `C ⊆ V` of size `|C| ≤ k'` such that every edge `(u, v) ∈ E` has at least one endpoint in `C` (`u ∈ C` or `v ∈ C`)?

2. **Theorem**:
   For any undirected graph `G = (V, E)` and any subset `S ⊆ V`:
   `S` is an **Independent Set** in `G` if and only if `V - S` is a **Vertex Cover** in `G`.

   *Proof (⇒)*:
   Suppose `S` is an independent set in `G`.
   Let `(u, v)` be an arbitrary edge in `E`.
   By the definition of an independent set, vertices `u` and `v` cannot both be in `S`.
   Therefore, at least one of `u` or `v` must NOT be in `S`, meaning at least one of `u` or `v` must belong to `V - S`.
   Since this holds for every edge in `E`, the set `C = V - S` covers every edge in `G`.
   Thus, `V - S` is a valid Vertex Cover.

   *Proof (⇐)*:
   Suppose `V - S` is a vertex cover in `G`.
   Suppose for contradiction that `S` is NOT an independent set.
   Then there must exist two vertices `u, v ∈ S` such that `(u, v) ∈ E`.
   Since both `u` and `v` belong to `S`, neither `u` nor `v` belongs to `V - S`.
   This means the edge `(u, v)` has neither endpoint in `V - S`, which contradicts the fact that `V - S` is a vertex cover.
   Therefore, no edge can connect two vertices in `S`. Thus, `S` is an Independent Set.

3. **Polynomial Equivalence**:
   To solve `IS(G, k)`: Ask if `VC(G, |V| - k)` is YES.
   To solve `VC(G, k')`: Ask if `IS(G, |V| - k')` is YES.
   The transformation takes `O(1)` time. Hence, `IS ≤_p VC` and `VC ≤_p IS`.

---

### Q7 Solution: Reduction: 3-SAT ≤_p Clique
**Marking Scheme**:
- Graph construction and triad definition: 4 Marks
- Forward proof (Satisfiable ⟹ Clique of size m): 2 Marks
- Reverse proof (Clique of size m ⟹ Satisfiable): 2 Marks

**Answer**:
1. **Construction**:
   Let `φ = C1 ∧ C2 ∧ ... ∧ Cm` be a 3-CNF boolean formula with `m` clauses, where clause `C_r = (l_r1 ∨ l_r2 ∨ l_r3)`.
   Construct an undirected graph `G = (V, E)`:
   - For each clause `C_r`, create a triad of 3 vertices corresponding to its literals: `v_r1, v_r2, v_r3`. Total vertices `|V| = 3m`.
   - Place an edge between vertex `u` (literal `l_ri` in clause `C_r`) and vertex `v` (literal `l_sj` in clause `C_s`) if and only if:
     1. `r ≠ s` (they belong to **different clauses**).
     2. `l_ri ≠ ¬l_sj` (the literals are **consistent**; neither is the negation of the other).
   - Set the target clique size `k = m`.
   - Running time: `|V| = 3m` and `|E| ≤ (3m)²/2 = O(m²)`, which is polynomial.

2. **Forward Proof (φ is Satisfiable ⟹ G has Clique of size m)**:
   If `φ` is satisfiable, there exists a truth assignment that makes at least one literal in each clause TRUE.
   For each clause `C_r` (`r = 1, ..., m`), pick one literal that evaluates to TRUE under this assignment, and select its corresponding vertex in `G`.
   This gives a subset `S` of exactly `m` vertices.
   Now consider any two selected vertices `u, v ∈ S`:
   - They come from different clauses by construction.
   - Both correspond to literals that are TRUE under the same assignment, so they cannot be negations of each other (`xi` and `¬xi`).
   Therefore, an edge exists between every pair of vertices in `S`.
   Thus, `S` forms a **clique of size m** in `G`.

3. **Reverse Proof (G has Clique of size m ⟹ φ is Satisfiable)**:
   Suppose `G` contains a clique `K` of size `m`.
   Since vertices in the same triad have no edges between them, the clique cannot contain more than one vertex from any single clause triad.
   Because `|K| = m` and there are `m` triads, `K` must contain **exactly one vertex from each clause triad**.
   Furthermore, because all vertices in `K` are mutually connected by edges, no two vertices in `K` can represent contradictory literals (`xi` and `¬xi`).
   Construct a truth assignment:
   - Set every literal appearing in `K` to TRUE.
   - Set any unassigned variables to arbitrary values (e.g., FALSE).
   Since `K` contains one vertex from every clause triad, this assignment satisfies at least one literal in every clause.
   Therefore, all `m` clauses evaluate to TRUE, and `φ` is satisfiable.

---

### Q8 Solution: Linear Programming vs ILP & Formulations
**Marking Scheme**:
- (a) Distinction between LP and ILP: 3 Marks
- (b) Vertex Cover 0-1 ILP formulation: 3 Marks
- (c) 0/1 Knapsack 0-1 ILP formulation: 2 Marks

**Answer**:
**(a) Distinction between LP and ILP**:
- **Linear Programming (LP)**: Continuous variables `x_i ∈ ℝ`. Constraints and objective are linear. Solvable in **polynomial time** (`LP ∈ P`) via Karmarkar's or Ellipsoid algorithms.
- **Integer Linear Programming (ILP)**: Some or all variables are restricted to **integers** `x_i ∈ ℤ` or binary values `x_i ∈ {0, 1}`. ILP is **NP-Hard**. The integrality constraint breaks convexity, turning the feasible region into discrete lattice points.

**(b) Vertex Cover 0-1 ILP Formulation**:
Given graph `G = (V, E)`:
- **Decision Variables**: For each vertex `v ∈ V`, define binary variable `x_v ∈ {0, 1}`:
  `x_v = 1` if vertex `v` is included in the vertex cover, `0` otherwise.
- **Objective Function**:
  ```
  Minimize:  Σ x_v   (summed over all v ∈ V)
  ```
- **Constraints**:
  Every edge `(u, v) ∈ E` must have at least one endpoint chosen:
  ```
  x_u + x_v ≥ 1      for all (u, v) ∈ E
  x_v ∈ {0, 1}       for all v ∈ V
  ```

**(c) 0/1 Knapsack 0-1 ILP Formulation**:
Given `n` items with values `v_i`, weights `w_i`, and capacity `W`:
- **Decision Variables**: `x_i ∈ {0, 1}` for `i = 1, ..., n` (`x_i = 1` if item `i` is picked).
- **Objective Function**:
  ```
  Maximize:  Σ (v_i · x_i)   (for i = 1 to n)
  ```
- **Constraints**:
  ```
  Σ (w_i · x_i) ≤ W
  x_i ∈ {0, 1}              for all i = 1 to n
  ```

---

## Section C Solutions (10 Marks Each)

### Q9 Solution: Approximation Algorithms & 2-Approx Vertex Cover
**Marking Scheme**:
- (a) Definition of approximation algorithm and approximation ratio: 3 Marks
- (b) Maximal matching algorithm pseudocode: 3 Marks
- (c) Mathematical proof that ratio ρ = 2: 4 Marks

**Answer**:
**(a) Definition & Approximation Ratio**:
An **approximation algorithm** is a polynomial-time heuristic for an NP-Hard optimization problem that produces a feasible solution guaranteed to be within a provable factor of the optimal solution.
The **approximation ratio `ρ(n)`** is defined as:
```
max( C / C*, C* / C ) ≤ ρ(n)
```
- For a **minimization problem**: `C ≤ ρ(n) · C*`, where `C` is the algorithm's cost, `C*` is optimal, and `ρ(n) ≥ 1`.
- For a **maximization problem**: `C ≥ (1 / ρ(n)) · C*`.

**(b) 2-Approximation Algorithm for Vertex Cover (Maximal Matching)**:
```text
Algorithm Approx_Vertex_Cover(G = (V, E)):
1. Initialize C = ∅
2. Initialize E' = E
3. While E' ≠ ∅:
       Pick an arbitrary edge (u, v) from E'
       Add BOTH u and v to C:  C = C ∪ {u, v}
       Remove from E' every edge incident to u or v
4. Return C
```

**(c) Rigorous Proof that Approximation Ratio ρ = 2**:
1. **Validity of Vertex Cover**:
   The algorithm terminates only when `E'` is empty. Because every edge removed from `E'` is incident to at least one vertex placed in `C`, every edge in `G` is covered. Thus, `C` is a valid vertex cover.
2. **Matching Property**:
   Let `M` be the set of edges selected in line 3. By design, whenever edge `(u, v)` is picked, all other edges sharing `u` or `v` are eliminated from `E'`. Thus, no two edges in `M` share an endpoint. `M` is a **maximal matching**.
3. **Size of Generated Cover |C|**:
   Since both endpoints of each edge in `M` are added to `C`:
   ```
   |C| = 2 · |M|
   ```
4. **Lower Bound on Optimal Cover |C*|**:
   Any valid vertex cover must cover every edge in the graph, including the edges in `M`.
   Because the edges in `M` are pairwise vertex-disjoint (no shared vertices), no single vertex can cover more than one edge of `M`.
   Therefore, to cover the `|M|` independent edges of `M`, any valid vertex cover (including the minimum/optimal cover `C*`) must select at least one unique vertex per edge:
   ```
   |C*| ≥ |M|
   ```
5. **Ratio Bound**:
   Substituting `|M| ≤ |C*|` into the size of `C`:
   ```
   |C| = 2 · |M| ≤ 2 · |C*|
   ```
   Dividing by `|C*|`:
   ```
   |C| / |C*| ≤ 2
   ```
   Therefore, the algorithm is a polynomial-time **2-approximation algorithm** running in `O(V + E)` time.

---

### Q10 Solution: 2-Approximation for Metric TSP
**Marking Scheme**:
- (a) Metric TSP definition and Triangle Inequality: 2 Marks
- (b) MST doubling and shortcutting algorithm: 4 Marks
- (c) Step-by-step cost proof showing Cost(H) ≤ 2 · Cost(H*): 4 Marks

**Answer**:
**(a) Metric TSP & Triangle Inequality**:
The Traveling Salesperson Problem (TSP) seeks a Hamiltonian cycle of minimum total edge weight. In **Metric TSP**, edge weights represent distances satisfying the **Triangle Inequality**:
```
c(u, w) ≤ c(u, v) + c(v, w)   for all u, v, w ∈ V
```
*(Direct path between u and w is always as short as or shorter than any path detour through v).*

**(b) The 2-Approximation Algorithm**:
1. **Step 1**: Construct a Minimum Spanning Tree (MST) `T` of graph `G` using Prim's or Kruskal's algorithm.
2. **Step 2**: Double every edge of `T` to form a multigraph `G'`. In `G'`, every vertex has an even degree.
3. **Step 3**: Find an **Eulerian Tour** `W` on `G'` that traverses every edge of `G'` exactly once.
4. **Step 4**: Construct a Hamiltonian cycle `H` by performing a traversal along `W` and **shortcutting** (skipping) any vertex that has already been visited, returning to the start vertex.

**(c) Proof that Approximation Ratio is 2**:
1. **Lower Bound via MST**:
   Let `H*` be the optimal TSP tour.
   If we remove any one edge from `H*`, the remaining edges form a spanning tree of `G`.
   Since `T` is a *minimum* spanning tree, its weight cannot exceed the weight of any other spanning tree:
   ```
   Cost(T) ≤ Cost(H* with one edge removed) < Cost(H*)
   Cost(T) < Cost(H*)
   ```
2. **Cost of the Eulerian Tour W**:
   Since `W` contains exactly two copies of every edge in `T`:
   ```
   Cost(W) = 2 · Cost(T)
   ```
   Combining with the MST lower bound:
   ```
   Cost(W) = 2 · Cost(T) < 2 · Cost(H*)
   ```
3. **Shortcutting via Triangle Inequality**:
   The Hamiltonian cycle `H` is obtained by replacing subpaths of the form `u → v1 → v2 → ... → w` (where intermediate vertices were previously visited) with direct shortcut edges `u → w`.
   By repeated application of the triangle inequality:
   ```
   c(u, w) ≤ c(u, v1) + c(v1, v2) + ... + c(v_k, w)
   ```
   Therefore, shortcutting never increases the total cost:
   ```
   Cost(H) ≤ Cost(W)
   ```
4. **Final Bound**:
   ```
   Cost(H) ≤ Cost(W) = 2 · Cost(T) < 2 · Cost(H*)
   ```
   Hence, `Cost(H) ≤ 2 · Cost(H*)`. The algorithm guarantees a solution cost at most **2× optimal** in `O(E log V)` time.

---

### Q11 Solution: The Halting Problem & Undecidability
**Marking Scheme**:
- (a) Formal definition of Halting Problem: 2 Marks
- (b) Proof of undecidability via Turing's diagonalization: 5 Marks
- (c) Explanation of why it is NP-Hard and not in NP: 3 Marks

**Answer**:
**(a) Formal Definition**:
The **Halting Problem** is the decision problem: Given a description of an arbitrary computer program / Turing machine `M` and an input `w`, determine whether `M` will eventually halt when run on input `w`, or run forever in an infinite loop.
Language: `H_TM = { <M, w> : M is a TM and M halts on input w }`.

**(b) Proof of Undecidability (Alan Turing, 1936)**:
*Proof by Contradiction*:
1. Assume there exists an algorithm (decider) `Halt(P, x)` that solves the Halting Problem:
   ```text
   Halt(P, x) = TRUE   if program P halts on input x
   Halt(P, x) = FALSE  if program P loops forever on input x
   ```
2. Using `Halt`, we can construct a new paradoxical program called `Diagonal(P)`:
   ```text
   function Diagonal(P):
       if Halt(P, P) == TRUE:
           while (TRUE) { }   // Loop infinitely!
       else:
           return TRUE        // Halt immediately!
   ```
3. Now consider what happens when `Diagonal` is fed its own code as input: `Diagonal(Diagonal)`.
   - **Case 1**: `Diagonal(Diagonal)` halts.
     Then `Halt(Diagonal, Diagonal)` must return `TRUE`.
     Looking at the code of `Diagonal`, if `Halt` returns `TRUE`, `Diagonal` enters an infinite while loop and does NOT halt. Contradiction!
   - **Case 2**: `Diagonal(Diagonal)` loops forever.
     Then `Halt(Diagonal, Diagonal)` must return `FALSE`.
     Looking at the code of `Diagonal`, if `Halt` returns `FALSE`, `Diagonal` returns `TRUE` and halts immediately. Contradiction!
4. In both mutually exclusive cases, we arrive at an impossible logical contradiction.
5. Therefore, the initial assumption that the algorithm `Halt` exists must be false. The Halting Problem is **undecidable**.

**(c) Why Halting is NP-Hard but NOT in NP**:
- **Why it is NP-Hard**: Any problem `L ∈ NP` can be solved by an algorithm that halts in polynomial time. We can reduce any problem `L ∈ NP` to the Halting Problem by constructing a machine that halts if and only if the instance is in `L`. Thus, `L ≤_p H_TM` for all `L ∈ NP`.
- **Why it is NOT in NP**: Any problem in NP must be verifiable in deterministic polynomial time. Since the Halting Problem cannot be decided or verified by any algorithm in any finite amount of time (it is uncomputable), it does NOT belong to NP.

---

### Q12 Solution: Karger's Randomized Min-Cut Algorithm
**Marking Scheme**:
- (a) Contraction procedure: 3 Marks
- (b) Success probability derivation P ≥ 2 / n²: 4 Marks
- (c) Error reduction through independent repetition: 3 Marks

**Answer**:
**(a) Algorithm Description**:
Let `G = (V, E)` be an undirected connected multigraph.
```text
Algorithm Karger_MinCut(G):
While |V| > 2:
    Pick an edge e = (u, v) uniformly at random from E
    Contract edge e: merge vertices u and v into a single super-vertex
    Remove any self-loops created by contraction
    Keep all parallel edges between distinct vertices
Return the cut formed by the edges connecting the remaining 2 super-vertices
```

**(b) Success Probability Derivation**:
Let `C` be a specific minimum cut of size `k` (`|C| = k`).
1. Every vertex in `G` must have degree at least `k` (otherwise an individual vertex would define a cut smaller than `k`).
2. Sum of degrees: `Σ deg(v) = 2 · |E| ≥ n · k ⟹ |E| ≥ n · k / 2`.
3. In the first step, a randomly chosen edge is in the min-cut `C` with probability:
   ```
   P(e ∈ C) = |C| / |E| ≤ k / (n · k / 2) = 2 / n
   ```
4. Therefore, the probability that the first contraction does NOT destroy cut `C` is:
   ```
   P(Step 1 succeeds) = 1 - 2/n = (n - 2) / n
   ```
5. In general, at step `i` (when there are `n - i + 1` vertices remaining):
   ```
   P(Step i succeeds | previous succeeded) ≥ 1 - 2 / (n - i + 1) = (n - i - 1) / (n - i + 1)
   ```
6. The algorithm contracts edges until 2 vertices remain (`n - 2` contractions). The overall probability of success is the product of all conditional probabilities (telescoping product):
   ```
   P(Success) ≥ ((n-2)/n) · ((n-3)/(n-1)) · ((n-4)/(n-2)) · ... · (2/4) · (1/3)
   ```
   All intermediate numerator terms cancel with subsequent denominator terms:
   ```
   P(Success) ≥ (2 · 1) / (n · (n - 1)) = 2 / (n · (n - 1)) ≥ 2 / n²
   ```

**(c) Error Probability Reduction by Repetition**:
A single run has failure probability `≤ 1 - 2/n²`.
If we run Karger's algorithm independently `k = (n² / 2) · ln(n)` times and choose the smallest cut found:
```
P(All k runs fail) ≤ (1 - 2/n²)^k
```
Using the standard inequality `1 - x ≤ e^(-x)` where `x = 2/n²`:
```
P(Failure) ≤ ( e^(-2/n²) )^( (n²/2) · ln n ) = e^(-ln n) = 1 / n
```
Thus, with `O(n² ln n)` runs, the probability of finding the global minimum cut is **≥ 1 - 1/n** (exceedingly high probability).
