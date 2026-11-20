# DAA Unit V: Network Flows Optimization — Complete Solutions & Marking Scheme

**Course Code**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Sources Used**: `DAA_Question_Bank_Ans_Final.docx`, `DAAOA_Question_Bank.docx`, `Syllabus_CS3212.pdf`  
**Mathematical Notation**: Universal Unicode text symbols.

---

## Section A Solutions (4 Marks Each)

### Q1 Solution: Definition and Properties of Flow Networks
**Marking Scheme**:
- Definition of Flow Network with source and sink: 1 Mark
- Three properties correctly stated with equations: 3 Marks (1 Mark each)

**Answer**:
A **flow network** is a directed graph G = (V, E) where each edge (u, v) ∈ E has a non-negative real capacity c(u, v) ≥ 0, containing two distinguished vertices: a source `s` and a sink `t`.

A valid **flow** is a function f: V × V → ℝ satisfying:
1. **Capacity Constraint**: `0 ≤ f(u, v) ≤ c(u, v)` for all u, v ∈ V. (Flow cannot exceed edge capacity).
2. **Skew Symmetry**: `f(u, v) = -f(v, u)` for all u, v ∈ V. (Flow from u to v is the negative of reverse flow).
3. **Flow Conservation**: For every vertex `u ∈ V - {s, t}`:
   `Σ f(u, v) = 0` (summed over all v ∈ V). Total incoming flow equals total outgoing flow.

---

### Q2 Solution: Residual Network Concept & Capacity Calculation
**Marking Scheme**:
- Concept of Residual Network G_f: 1 Mark
- Forward edge residual capacity formula and value: 1.5 Marks
- Backward edge residual capacity formula and value: 1.5 Marks

**Answer**:
A **residual network** `G_f = (V, E_f)` represents the available capacity to push additional flow in a network G with current flow f.

Given `c(u, v) = 15` and `f(u, v) = 9`:
1. **Forward Edge `(u, v)`**:
   - `c_f(u, v) = c(u, v) - f(u, v) = 15 - 9 = 6`.
   - *Physical Meaning*: We can push up to 6 additional units of flow directly from u to v without violating the capacity constraint.
2. **Backward Edge `(v, u)`**:
   - `c_f(v, u) = f(u, v) = 9`.
   - *Physical Meaning*: We can cancel or undo up to 9 units of flow previously routed from u to v by redirecting flow backwards from v to u.

---

### Q3 Solution: Cuts, Net Flow, and Cut Capacity
**Marking Scheme**:
- Definition of cut (S, T): 1 Mark
- Net flow formula f(S, T): 1 Mark
- Capacity formula c(S, T): 1 Mark
- Common student mistake regarding reverse edges: 1 Mark

**Answer**:
1. **Cut (S, T)**: A partition of vertex set V into two disjoint sets S and T = V - S such that source `s ∈ S` and sink `t ∈ T`.
2. **Net Flow `f(S, T)`**:
   `f(S, T) = Σ Σ f(u, v)` for all u ∈ S, v ∈ T. By flow conservation, `f(S, T) = |f|` (the total flow value).
3. **Capacity of Cut `c(S, T)`**:
   `c(S, T) = Σ Σ c(u, v)` for all u ∈ S, v ∈ T.
4. **Common Student Mistake**:
   Students frequently add the capacities of reverse edges `(v, u)` where `v ∈ T` and `u ∈ S` into `c(S, T)`. Reverse edges are **STRICTLY EXCLUDED** from the cut capacity. Only edges directed from S into T contribute to capacity.

---

### Q4 Solution: Ford-Fulkerson Pseudo-Polynomial Nature & Failure
**Marking Scheme**:
- Explanation of pseudo-polynomial time: 2 Marks
- Pathological scenario with diagram/example: 2 Marks

**Answer**:
1. **Pseudo-polynomial Time**:
   Ford-Fulkerson runs in `O(E · |f*|)` time where `|f*|` is the numerical value of maximum flow. Because the running time depends on the numeric magnitude of the input value rather than the length of the binary representation of the input, it is pseudo-polynomial.
2. **Pathological Scenario**:
   Consider a diamond network with nodes `{s, u, v, t}`:
   - `c(s, u) = 1,000,000`, `c(s, v) = 1,000,000`
   - `c(u, t) = 1,000,000`, `c(v, t) = 1,000,000`
   - Middle bridge edge `(u, v)` has capacity `c(u, v) = 1`.
   If DFS repeatedly chooses augmenting paths that alternate across the bridge:
   - Iteration 1: `s → u → v → t` pushes bottleneck = 1.
   - Iteration 2: `s → v → u → t` (via backward edge `v → u`) pushes bottleneck = 1.
   The algorithm takes **2,000,000 iterations** to find the max flow of 2,000,000, each time augmenting by only 1 unit!

---

## Section B Solutions (8 Marks Each)

### Q5 Solution: Max-Flow Min-Cut Theorem & Procedure
**Marking Scheme**:
- (a) Theorem statement and 3 equivalent conditions: 4 Marks
- (b) Systematic procedure to find min-cut: 4 Marks

**Answer**:
**(a) Statement and Equivalent Conditions**:
For any flow network G = (V, E) with source `s`, sink `t`, and flow `f`, the following three statements are strictly equivalent:
1. `f` is a maximum flow in G.
2. The residual network `G_f` contains no augmenting paths from `s` to `t`.
3. `|f| = c(S, T)` for some cut `(S, T)` of G (and this cut is a minimum cut).

**(b) Procedure to Identify the Minimum Cut**:
1. Run the Ford-Fulkerson or Edmonds-Karp algorithm until termination (no path from s to t in G_f).
2. Construct the final residual graph `G_f`.
3. Perform a graph traversal (BFS or DFS) starting from source `s` in `G_f`, following only edges with residual capacity `c_f(u, v) > 0`.
4. Define the vertex subset `S`:
   `S = { v ∈ V : there exists a directed path from s to v in G_f }`.
5. Define the vertex subset `T`:
   `T = V - S`.
6. Since there is no path from `s` to `t` in `G_f`, the sink `t ∉ S`, so `t ∈ T`. Thus `(S, T)` forms a valid cut.
7. Every edge `(u, v)` with `u ∈ S` and `v ∈ T` has `c_f(u, v) = 0`, meaning it is fully saturated: `f(u, v) = c(u, v)`.
8. The capacity `c(S, T) = Σ c(u, v)` for `u ∈ S, v ∈ T` equals `|f*|`. This cut `(S, T)` is guaranteed to be a Minimum Cut.

---

### Q6 Solution: Comparison Table: Ford-Fulkerson vs Edmonds-Karp
**Marking Scheme**:
- Structured table with all 5 points correctly filled: 8 Marks (approx 1.5M per row)

**Answer**:

| Comparison Parameter | Ford-Fulkerson Method | Edmonds-Karp Algorithm |
| :--- | :--- | :--- |
| **Augmenting Path Criterion** | Any arbitrary path from `s` to `t` in `G_f` | Shortest path from `s` to `t` by hop count (fewest edges) |
| **Search Algorithm Used** | Typically Depth-First Search (DFS) or arbitrary search | Exclusively Breadth-First Search (BFS) |
| **Time Complexity** | `O(E · \|f*\|)` | `O(V · E²)` |
| **Complexity Class** | Pseudo-polynomial (depends on flow value `\|f*\|`) | Strongly polynomial (depends only on `\|V\|` and `\|E\|`) |
| **Dependence on Capacities** | Running time grows linearly with numeric capacity values | Runtime is independent of edge capacity magnitudes |
| **Irrational Capacities** | May fail to terminate or converge to a non-maximal value | Always terminates and guarantees maximum flow |
| **Maximum Augmentations** | Up to `\|f*\|` augmentations | At most `O(V · E)` total augmentations |

---

### Q7 Solution: Maximum Bipartite Matching via Max Flow
**Marking Scheme**:
- Network transformation steps: 2.5 Marks
- Capacities assignment: 1.5 Marks
- Integrality Theorem and validity proof: 2.5 Marks
- Complexity derivation: 1.5 Marks

**Answer**:
1. **Network Transformation**:
   Let `G = (V, E)` be a bipartite graph with vertex partition `V = L ∪ R`.
   Construct a directed flow network `G' = (V', E')` as follows:
   - Add a supersource `s` and a supersink `t`: `V' = L ∪ R ∪ {s, t}`.
   - For every vertex `u ∈ L`, add a directed edge `(s, u)`.
   - For every original undirected edge `(u, v) ∈ E` where `u ∈ L, v ∈ R`, add a directed edge `(u, v)`.
   - For every vertex `v ∈ R`, add a directed edge `(v, t)`.

2. **Capacities Assigned**:
   - `c(s, u) = 1` for all `u ∈ L`.
   - `c(u, v) = 1` (or ∞) for all `(u, v) ∈ E`.
   - `c(v, t) = 1` for all `v ∈ R`.

3. **Integrality Theorem & Proof of Matching**:
   - *Theorem*: If all edge capacities in a flow network are integers, then the maximum flow produced by Ford-Fulkerson assigns an integer flow `f(u, v)` to every edge.
   - Since all capacities in `G'` are 1, each edge carries either `0` or `1` unit of flow: `f(u, v) ∈ {0, 1}`.
   - Because `c(s, u) = 1`, by flow conservation at most 1 unit can leave vertex `u ∈ L`. Thus, at most one outgoing edge `(u, v)` carries flow 1.
   - Because `c(v, t) = 1`, by flow conservation at most 1 unit can enter vertex `v ∈ R`. Thus, at most one incoming edge `(u, v)` carries flow 1.
   - Therefore, the edge subset `M = { (u, v) ∈ E : f(u, v) = 1 }` has no two edges sharing a common vertex. `M` is a valid matching in G.
   - The value of flow `|f| = Σ f(s, u) = |M|`. Maximizing flow maximizes matching size: `|M_max| = |f*|`.

4. **Time Complexity**:
   - `|V'| = |V| + 2` and `|E'| = |E| + |V|`.
   - The maximum matching size is at most `min(|L|, |R|) ≤ |V|`.
   - Each augmentation takes `O(E') = O(E)` time.
   - Total Time = `|V| · O(E) = O(V · E)`.

---

### Q8 Solution: Proof of Weak Duality & Optimality Criterion
**Marking Scheme**:
- Proof that f(S, T) = |f|: 3 Marks
- Proof that f(S, T) ≤ c(S, T): 2 Marks
- Deduction of optimality when |f| = c(S*, T*): 3 Marks

**Answer**:
1. **Proof that `f(S, T) = |f|`**:
   The value of flow is `|f| = Σ f(s, v)` for all `v ∈ V`.
   For any intermediate vertex `u ∈ S - {s}`, flow conservation dictates `Σ f(u, v) = 0`.
   Summing over all `u ∈ S`:
   `Σ_{u ∈ S} Σ_{v ∈ V} f(u, v) = Σ_{v ∈ V} f(s, v) + Σ_{u ∈ S - {s}} 0 = |f|`.
   Partition `V` into `S` and `T`:
   `Σ_{u ∈ S} Σ_{v ∈ V} f(u, v) = Σ_{u ∈ S} [ Σ_{v ∈ S} f(u, v) + Σ_{v ∈ T} f(u, v) ]`.
   By skew symmetry, `f(u, v) = -f(v, u)`, so all internal edges within S cancel out pairwise: `Σ_{u ∈ S} Σ_{v ∈ S} f(u, v) = 0`.
   Therefore:
   `|f| = Σ_{u ∈ S} Σ_{v ∈ T} f(u, v) = f(S, T)`.

2. **Proof that `f(S, T) ≤ c(S, T)`**:
   For each individual edge `(u, v)`:
   By the capacity constraint: `f(u, v) ≤ c(u, v)`.
   Therefore:
   `f(S, T) = Σ_{u ∈ S, v ∈ T} f(u, v) ≤ Σ_{u ∈ S, v ∈ T} c(u, v) = c(S, T)`.
   Hence, `|f| ≤ c(S, T)`.

3. **Optimality Deduction**:
   Let `f*` be a maximum flow and `(S_min, T_min)` be a minimum cut.
   For ANY flow `f` and ANY cut `(S, T)`:
   `|f| ≤ |f*| ≤ c(S_min, T_min) ≤ c(S, T)`.
   If for some specific flow `f` and cut `(S*, T*)` we have `|f| = c(S*, T*)`, then:
   No flow can have value greater than `c(S*, T*)` (since all flows are bounded by any cut capacity), so `f` MUST be a **maximum flow**.
   Similarly, no cut can have capacity less than `|f|` (since all cuts upper-bound all flows), so `(S*, T*)` MUST be a **minimum cut**.

---

## Section C Solutions (10 Marks Each)

### Q9 Solution: Ford-Fulkerson Full Step-by-Step Numerical
**Marking Scheme**:
- Initial state and Path 1 with update: 2 Marks
- Path 2 and Path 3 with updates: 2 Marks
- Path 4 with update: 1.5 Marks
- Path 5 (using backward edge) with update: 2 Marks
- Final flow value and verification: 1 Mark
- Minimum Cut identification and capacity calculation: 1.5 Marks

**Answer**:
Given capacities:
`s → A: 10`, `s → B: 10`, `A → B: 2`, `A → C: 4`, `A → D: 8`, `B → D: 9`, `C → t: 10`, `D → C: 6`, `D → t: 10`.

#### Step 1: Initial Flow
All edge flows `f = 0`. Net flow `|f| = 0`.

#### Step 2: Augmenting Paths

**Iteration 1: Path P1 = s → A → D → t**
- Residual capacities: `c_f(s, A) = 10`, `c_f(A, D) = 8`, `c_f(D, t) = 10`.
- Bottleneck `c_f(P1) = min(10, 8, 10) = 8`.
- Flow update: `f(s, A) = 8`, `f(A, D) = 8`, `f(D, t) = 8`.
- Total flow `|f| = 8`.

**Iteration 2: Path P2 = s → B → D → C → t**
- Residual capacities: `c_f(s, B) = 10`, `c_f(B, D) = 9`, `c_f(D, C) = 6`, `c_f(C, t) = 10`.
- Bottleneck `c_f(P2) = min(10, 9, 6, 10) = 6`.
- Flow update: `f(s, B) = 6`, `f(B, D) = 6`, `f(D, C) = 6`, `f(C, t) = 6`.
- Total flow `|f| = 8 + 6 = 14`.

**Iteration 3: Path P3 = s → A → C → t**
- Residual capacities: `c_f(s, A) = 10 - 8 = 2`, `c_f(A, C) = 4`, `c_f(C, t) = 10 - 6 = 4`.
- Bottleneck `c_f(P3) = min(2, 4, 4) = 2`.
- Flow update: `f(s, A) = 8 + 2 = 10` *(saturated)*, `f(A, C) = 2`, `f(C, t) = 6 + 2 = 8`.
- Total flow `|f| = 14 + 2 = 16`.

**Iteration 4: Path P4 = s → B → D → t**
- Residual capacities: `c_f(s, B) = 10 - 6 = 4`, `c_f(B, D) = 9 - 6 = 3`, `c_f(D, t) = 10 - 8 = 2`.
- Bottleneck `c_f(P4) = min(4, 3, 2) = 2`.
- Flow update: `f(s, B) = 6 + 2 = 8`, `f(B, D) = 6 + 2 = 8`, `f(D, t) = 8 + 2 = 10` *(saturated)*.
- Total flow `|f| = 16 + 2 = 18`.

**Iteration 5: Path P5 = s → B → D → A → C → t (Using Backward Edge D → A!)**
- Residual capacities:
  - `c_f(s, B) = 10 - 8 = 2`
  - `c_f(B, D) = 9 - 8 = 1`
  - `c_f(D, A) = f(A, D) = 8` *(backward edge!)*
  - `c_f(A, C) = 4 - 2 = 2`
  - `c_f(C, t) = 10 - 8 = 2`
- Bottleneck `c_f(P5) = min(2, 1, 8, 2, 2) = 1`.
- Flow update:
  - `f(s, B) = 8 + 1 = 9`
  - `f(B, D) = 8 + 1 = 9` *(saturated)*
  - `f(A, D) = 8 - 1 = 7` *(flow reduced on A → D)*
  - `f(A, C) = 2 + 1 = 3`
  - `f(C, t) = 8 + 1 = 9`
- Total flow `|f| = 18 + 1 = 19`.

#### Step 3: Termination & Reachability
In G_f, traverse from `s`:
- `c_f(s, A) = 10 - 10 = 0` (cannot reach A)
- `c_f(s, B) = 10 - 9 = 1 > 0` (can reach B)
- From B, `c_f(B, D) = 9 - 9 = 0` (cannot reach D).
No other outgoing residual edges from {s, B}.
Therefore, sink `t` is unreachable. **Algorithm terminates.**

#### Step 4: Final Results & Min Cut
1. **Maximum Flow Value**: `|f*| = 19`.
2. **Cut Sets**:
   - `S = { s, B }`
   - `T = { A, C, D, t }`
3. **Cut Capacity `c(S, T)`**:
   Edges crossing from S to T:
   - `(s, A)`: capacity = 10
   - `(B, D)`: capacity = 9
   - `c(S, T) = 10 + 9 = 19`.
4. **Verification**: `|f*| = 19 = c(S, T)`. Proved!

---

### Q10 Solution: Edmonds-Karp with BFS Shortest Paths
**Marking Scheme**:
- Step-by-step BFS augmenting paths (hop-count priority): 5 Marks
- Residual network updates: 3 Marks
- Final flow and Min-Cut verification: 2 Marks

**Answer**:
Nodes: `{s, 1, 2, 3, 4, t}`
Capacities: `c(s, 1)=16, c(s, 2)=13, c(1, 2)=10, c(1, 3)=12, c(2, 1)=4, c(2, 4)=14, c(3, 2)=9, c(3, t)=20, c(4, 3)=7, c(4, t)=4`.

#### Execution:
Edmonds-Karp uses BFS to select the augmenting path with the **minimum number of edges**.

- **Iteration 1**:
  BFS discovers 3-edge shortest path: `P1 = s → 1 → 3 → t` (3 edges).
  Residual capacities: `c_f(s, 1)=16`, `c_f(1, 3)=12`, `c_f(3, t)=20`.
  Bottleneck: `c_f(P1) = min(16, 12, 20) = 12`.
  Augment by 12: `f(s, 1)=12`, `f(1, 3)=12` (saturated), `f(3, t)=12`.
  Total flow `|f| = 12`.

- **Iteration 2**:
  BFS discovers shortest path: `P2 = s → 2 → 4 → t` (3 edges).
  Residual capacities: `c_f(s, 2)=13`, `c_f(2, 4)=14`, `c_f(4, t)=4`.
  Bottleneck: `c_f(P2) = min(13, 14, 4) = 4`.
  Augment by 4: `f(s, 2)=4`, `f(2, 4)=4`, `f(4, t)=4` (saturated).
  Total flow `|f| = 12 + 4 = 16`.

- **Iteration 3**:
  BFS discovers 4-edge path: `P3 = s → 2 → 4 → 3 → t` (4 edges).
  Residual capacities: `c_f(s, 2)=13-4=9`, `c_f(2, 4)=14-4=10`, `c_f(4, 3)=7`, `c_f(3, t)=20-12=8`.
  Bottleneck: `c_f(P3) = min(9, 10, 7, 8) = 7`.
  Augment by 7: `f(s, 2)=4+7=11`, `f(2, 4)=4+7=11`, `f(4, 3)=7` (saturated), `f(3, t)=12+7=19`.
  Total flow `|f| = 16 + 7 = 23`.

- **Iteration 4**:
  BFS discovers path: `P4 = s → 1 → 2 → 4 → 3 → t`? But `(4, 3)` is saturated.
  Check path: `P4 = s → 1 → 2 → t`? No direct edge `2 → t`.
  Check: `s → 1 → 2` (c_f = min(4, 10) = 4). From 2, `2 → 4` has spare capacity 3. But all edges out of 4 to t or 3 are full!
  Check backward edges: Can we reach 3?
  From 1, `(1, 3)` is full.
  From 2, `(2, 1)` can push 4? But 1 has no way to t.
  No augmenting path exists in `G_f`.

#### Min-Cut & Verification:
- Reachable from `s` in G_f:
  - From `s`: reach `1` (`c_f(s, 1) = 16 - 12 = 4 > 0`) and reach `2` (`c_f(s, 2) = 13 - 11 = 2 > 0`).
  - From `1`: reach `2` via forward edge `(1, 2)` (`c_f(1, 2) = 10 > 0`).
  - From `2`: reach `4` (`c_f(2, 4) = 14 - 11 = 3 > 0`).
  - From `4`: `(4, t)` is full (0), `(4, 3)` is full (0).
  - From `3`: not reachable.
  - Set `S = { s, 1, 2, 4 }`.
  - Set `T = { 3, t }`.
- Edges crossing S to T:
  - `(1, 3)`: capacity = 12
  - `(4, 3)`: capacity = 7
  - `(4, t)`: capacity = 4
  - `c(S, T) = 12 + 7 + 4 = 23`.
- **Max Flow = 23**, **Min Cut Capacity = 23**.

---

### Q11 Solution: University Teaching Assistant Bipartite Matching
**Marking Scheme**:
- Bipartite graph formulation: 3 Marks
- Max Flow reduction with capacities: 3 Marks
- Augmenting path execution and final matching: 4 Marks

**Answer**:
1. **Bipartite Graph Formulation**:
   - Left set `L = {T1, T2, T3, T4}` (TAs)
   - Right set `R = {C1, C2, C3, C4}` (Courses)
   - Edges `E = { (T1, C1), (T1, C2), (T2, C1), (T2, C3), (T3, C2), (T3, C3), (T3, C4), (T4, C3) }`.

2. **Flow Network Construction**:
   - Add supersource `s` and supersink `t`.
   - Directed edges `s → Ti` with `c(s, Ti) = 1` for all `i = 1..4`.
   - Directed edges `Ti → Cj` for each bipartite edge with `c(Ti, Cj) = 1`.
   - Directed edges `Cj → t` with `c(Cj, t) = 1` for all `j = 1..4`.

3. **Step-by-Step Augmentation**:
   - **Path 1**: `s → T1 → C1 → t` (bottleneck = 1).
     Flow: `f(T1, C1) = 1`. Course C1 matched to T1.
   - **Path 2**: `s → T2 → C3 → t` (bottleneck = 1).
     Flow: `f(T2, C3) = 1`. Course C3 matched to T2.
   - **Path 3**: `s → T3 → C2 → t` (bottleneck = 1).
     Flow: `f(T3, C2) = 1`. Course C2 matched to T3.
   - **Path 4**: Now consider `T4`. `T4` can only teach `C3`, but `C3` is already matched to `T2`!
     Search residual path in G_f:
     `s → T4 → C3 → T2 → C1 → T1 → C2 → T3 → C4 → t`.
     Let's check direct alternative:
     `T3` can teach `C4`.
     Augmenting path: `s → T4 → C3` (fwd) `→ T2` (back) `→ C1` (fwd) `→ T1` (back) `→ C2` (fwd) `→ T3` (back) `→ C4` (fwd) `→ t`!
     Or more directly:
     `T2` can teach `C1`, `T1` can teach `C2`, `T3` can teach `C4`, leaving `C3` for `T4`:
     - Match `T4` to `C3`
     - Match `T2` to `C1`
     - Match `T1` to `C2`
     - Match `T3` to `C4`
   - All 4 TAs and 4 Courses are matched!
4. **Final Maximum Matching (Size = 4)**:
   - `T1` → `C2`
   - `T2` → `C1`
   - `T3` → `C4`
   - `T4` → `C3`
   Maximum Matching cardinality = 4 (Complete / Perfect Matching).

---

### Q12 Solution: Edmonds-Karp Monotonicity & Critical Edge Proof
**Marking Scheme**:
- (a) Proof of Monotonic Distance Lemma: 6 Marks
- (b) Critical edge bound and O(V · E²) complexity derivation: 4 Marks

**Answer**:
**(a) Proof of Monotonic Distance Lemma**:
Let `δ_f(s, v)` denote the shortest-path distance (number of edges) from source `s` to vertex `v` in residual graph `G_f`.
We claim that after any flow augmentation producing residual graph `G_f'`, for all `v ∈ V`:
```
δ_f'(s, v) ≥ δ_f(s, v)
```
*Proof by contradiction*:
Suppose there exists a set of vertices for which `δ_f'(s, v) < δ_f(s, v)`.
Let `v` be a vertex in this set with the **minimum** `δ_f'(s, v)`.
Let `u` be the vertex immediately preceding `v` on a shortest path from `s` to `v` in `G_f'`.
Then:
```
δ_f'(s, v) = δ_f'(s, u) + 1
```
Because `δ_f'(s, u) < δ_f'(s, v)`, by our choice of `v`, vertex `u` does not violate monotonicity:
```
δ_f'(s, u) ≥ δ_f(s, u)
```
Now consider edge `(u, v)`:
- **Case 1: (u, v) was already an edge in G_f**.
  Then `δ_f(s, v) ≤ δ_f(s, u) + 1 ≤ δ_f'(s, u) + 1 = δ_f'(s, v)`.
  This contradicts `δ_f'(s, v) < δ_f(s, v)`.
- **Case 2: (u, v) was NOT an edge in G_f**.
  The edge `(u, v)` must have been created in `G_f'` by pushing flow along `(v, u)` during the augmentation.
  Because Edmonds-Karp uses shortest paths, the augmenting path used `(v, u)`, meaning:
  ```
  δ_f(s, u) = δ_f(s, v) + 1
  ```
  Substituting:
  `δ_f'(s, v) = δ_f'(s, u) + 1 ≥ δ_f(s, u) + 1 = (δ_f(s, v) + 1) + 1 = δ_f(s, v) + 2`.
  This implies `δ_f'(s, v) > δ_f(s, v)`, which again contradicts `δ_f'(s, v) < δ_f(s, v)`.
Therefore, shortest path distances never decrease: `δ_f'(s, v) ≥ δ_f(s, v)`.

**(b) Critical Edge Analysis & Complexity**:
1. An edge `(u, v)` on an augmenting path `P` is **critical** if its residual capacity equals the bottleneck: `c_f(u, v) = c_f(P)`.
2. When `(u, v)` is critical, it is saturated and disappears from the residual network.
3. At this moment:
   ```
   δ(s, v) = δ(s, u) + 1
   ```
4. Before `(u, v)` can reappear in a residual graph, flow must be augmented in the opposite direction along `(v, u)`.
   Let `δ'` be the distances when `(v, u)` is used:
   ```
   δ'(s, u) = δ'(s, v) + 1
   ```
   By monotonicity, `δ'(s, v) ≥ δ(s, v)`. Substituting:
   ```
   δ'(s, u) ≥ δ(s, v) + 1 = δ(s, u) + 1 + 1 = δ(s, u) + 2
   ```
5. Thus, between two consecutive occasions when `(u, v)` becomes critical, the distance from `s` to `u` increases by at least 2.
6. The distance is at most `|V| - 2` (since the path cannot exceed |V| vertices without a cycle).
7. Hence, edge `(u, v)` can become critical at most `(|V| - 2) / 2 < |V| / 2` times.
8. Since there are at most `2|E|` edges in the residual graph, the total number of critical edge occurrences (and hence the total number of flow augmentations) is at most:
   ```
   Total Augmentations ≤ 2 · |E| · (|V| / 2) = O(V · E)
   ```
9. Each augmentation uses BFS, which takes `O(E)` time.
10. **Overall Time Complexity**:
    ```
    Total Time = O(V · E) · O(E) = O(V · E²)
    ```
