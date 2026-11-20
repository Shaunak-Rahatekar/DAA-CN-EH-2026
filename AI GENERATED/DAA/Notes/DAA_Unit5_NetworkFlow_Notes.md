# DAA Unit V: Network Flows Optimization — Complete Exam Notes

**Course Code**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Sources Used**: `DAA_Question_Bank_Ans_Final.docx`, `DAAOA_Question_Bank.docx`, `Syllabus_CS3212.pdf`  
**Mathematical Notation**: Universal Unicode text symbols (no raw LaTeX tags).

---

## 1. Flow Networks: Fundamental Definitions & Properties

### 1.1 What is a Flow Network?
A **flow network** is a directed graph G = (V, E) where each directed edge (u, v) ∈ E has a non-negative **capacity** c(u, v) ≥ 0. If (u, v) ∉ E, we set c(u, v) = 0.
The network contains two distinguished vertices:
1. **Source vertex (s)**: Where material/flow originates (in-degree is typically 0).
2. **Sink vertex (t)**: Where material/flow terminates (out-degree is typically 0).
We assume every vertex v ∈ V lies on some directed path from s to t.

### 1.2 The Three Flow Constraints (Formal Properties)
A **flow** in G is a real-valued function f: V × V → ℝ satisfying three fundamental properties:

1. **Capacity Constraint**:
   For all u, v ∈ V:
   ```
   0 ≤ f(u, v) ≤ c(u, v)
   ```
   *Meaning*: The flow through an edge cannot be negative and cannot exceed the edge's capacity.

2. **Skew Symmetry**:
   For all u, v ∈ V:
   ```
   f(u, v) = - f(v, u)
   ```
   *Meaning*: Flow from u to v is the negative of flow from v to u. This simplifies mathematical accounting of net flow.

3. **Flow Conservation**:
   For all vertices u ∈ V - {s, t}:
   ```
   Σ f(u, v) = 0   (summed over all v ∈ V)
   ```
   Equivalently:
   ```
   Total Flow Entering u = Total Flow Leaving u
   ```
   *Meaning*: No intermediate node can store, create, or consume flow.

### 1.3 Value of a Flow
The **total value of a flow f**, denoted |f|, is defined as the net flow leaving the source:
```
|f| = Σ f(s, v)   (for all v ∈ V)
```
By flow conservation, this is identically equal to the net flow entering the sink t:
```
|f| = Σ f(v, t)   (for all v ∈ V)
```

---

## 2. Residual Networks & Augmenting Paths

### 2.1 Residual Network G_f
Given a flow network G = (V, E) and a current flow f, the **residual network** G_f = (V, E_f) represents the remaining available capacities for pushing additional flow.
The **residual capacity** c_f(u, v) of any pair of vertices is:
```
c_f(u, v) = c(u, v) - f(u, v)
```

There are two kinds of edges in G_f:
1. **Forward Edges**:
   If f(u, v) < c(u, v), we can push up to `c(u, v) - f(u, v)` additional units of flow along (u, v).
2. **Backward Edges**:
   If f(u, v) > 0, we can cancel (undo) up to `f(u, v)` units of flow by pushing flow backwards along (v, u).
   ```
   c_f(v, u) = c(v, u) - f(v, u) = 0 - (-f(u, v)) = f(u, v)
   ```
The residual edge set is:
```
E_f = { (u, v) ∈ V × V : c_f(u, v) > 0 }
```
Note that |E_f| ≤ 2 · |E|.

### 2.2 Augmenting Paths
An **augmenting path** P is a simple directed path from source s to sink t in the residual network G_f.
The maximum amount of additional flow that can be routed along P is the **bottleneck capacity** c_f(P):
```
c_f(P) = min { c_f(u, v) : (u, v) is an edge on P }
```

### 2.3 Flow Augmentation Rule
When an augmenting path P with bottleneck c_f(P) is found, the flow f is updated to a new flow f' = f ↑ f_P:
For each edge (u, v) along path P:
- If (u, v) is a **forward edge** in G: `f(u, v) ← f(u, v) + c_f(P)`
- If (u, v) is a **backward edge** (meaning (v, u) ∈ E): `f(v, u) ← f(v, u) - c_f(P)`
New total flow value:
```
|f'| = |f| + c_f(P)
```

---

## 3. Ford-Fulkerson Method & Edmonds-Karp Algorithm

### 3.1 The Ford-Fulkerson Method
The **Ford-Fulkerson method** is an iterative framework rather than a single specific algorithm because it does not specify how the augmenting path is chosen.

#### Algorithm Steps:
```text
Algorithm Ford_Fulkerson(G, s, t):
1. For each edge (u, v) in E:
       f(u, v) = 0
       f(v, u) = 0
2. While there exists an augmenting path P from s to t in G_f:
       c_f(P) = min { c_f(u, v) : (u, v) in P }
       For each edge (u, v) in P:
           f(u, v) = f(u, v) + c_f(P)
           f(v, u) = -f(u, v)
3. Return flow f
```

#### Complexity Analysis of Ford-Fulkerson:
- Finding a path using DFS or BFS takes `O(V + E) = O(E)` time (assuming connected graph where |E| ≥ |V| - 1).
- If edge capacities are integers, each augmentation increases the total flow |f| by at least 1 unit.
- Thus, the while-loop runs at most |f*| times, where |f*| is the maximum flow value.
- **Overall Time Complexity**: `O(E · |f*|)`.
- **Limitation / Pathological Case**:
  If capacities are large (e.g., c = 1,000,000) and DFS chooses poor augmenting paths (alternating back and forth), the algorithm can take 2,000,000 iterations.
  If capacities are irrational numbers, Ford-Fulkerson may not even terminate or may converge to an incorrect value!

### 3.2 The Edmonds-Karp Algorithm
The **Edmonds-Karp algorithm** is a concrete implementation of Ford-Fulkerson that resolves this limitation by specifying:
> **Rule**: Always choose the augmenting path P that has the **minimum number of edges** (shortest path in terms of hop count, not capacity) from s to t in the residual network G_f.
> This is implemented using **Breadth-First Search (BFS)**.

#### Key Theoretical Guarantees:
1. **Monotonic Distance Lemma**:
   For all vertices v ∈ V - {s, t}, the shortest-path distance δ_f(s, v) in the residual network G_f increases monotonically with each flow augmentation:
   ```
   δ_f'(s, v) ≥ δ_f(s, v)
   ```
2. **Critical Edge Bound**:
   An edge (u, v) on an augmenting path P is called **critical** if c_f(P) = c_f(u, v) (it equals the bottleneck).
   Once an edge becomes critical, it disappears from G_f. Before it can become critical again, flow must be pushed in the opposite direction along (v, u).
   Each edge can become critical at most `|V| / 2` times.
   Since there are at most 2|E| residual edges, the total number of flow augmentations is at most:
   ```
   Total Augmentations ≤ 2 · |E| · (|V| / 2) = O(V · E)
   ```
3. **Time Complexity**:
   - Each augmentation requires one BFS: `O(E)` time.
   - Total number of augmentations: `O(V · E)`.
   - **Total Edmonds-Karp Time Complexity**: `O(V · E · E) = O(V · E²)`.
   - This bound is **strongly polynomial**: it depends only on the number of vertices and edges, independent of edge capacity values!

---

## 4. Cuts in Flow Networks & Max-Flow Min-Cut Theorem

### 4.1 Definition of a Cut
A **cut** (S, T) in a flow network G = (V, E) is a partition of the vertex set V into two disjoint sets S and T = V - S such that:
```
s ∈ S   and   t ∈ T
```

### 4.2 Net Flow Across a Cut
The **net flow** across cut (S, T), denoted f(S, T), is the total flow leaving S minus the flow entering S:
```
f(S, T) = Σ Σ f(u, v)   (where u ∈ S, v ∈ T)
```
**Fundamental Lemma**: For any flow f and any cut (S, T), the net flow across the cut equals the total value of the flow:
```
f(S, T) = |f|
```
*Intuition*: All water leaving the source must eventually cross the boundary between S and T to reach the sink.

### 4.3 Capacity of a Cut
The **capacity** of a cut (S, T), denoted c(S, T), is the sum of capacities of edges directed from S to T **only**:
```
c(S, T) = Σ Σ c(u, v)   (where u ∈ S, v ∈ T)
```
> **EXAM TRAP / CRITICAL WARNING**:
> Edges directed backwards from T to S are **NOT** added to the cut capacity!
> Capacity counts only pipes leading from S into T.

### 4.4 Flow Upper Bound
Because f(u, v) ≤ c(u, v) for all forward edges, and f(u, v) ≥ 0:
```
|f| = f(S, T) ≤ c(S, T)
```
The value of ANY feasible flow is always bounded above by the capacity of ANY cut in the network.
Therefore:
```
Max Flow Value ≤ Capacity of Any Cut
Max Flow Value ≤ Min Cut Capacity
```

### 4.5 The Max-Flow Min-Cut Theorem
This is the central theorem of network flow theory.

#### Formal Statement:
If f is a flow in a network G = (V, E) with source s and sink t, then the following three statements are **strictly equivalent**:
1. `f` is a maximum flow in G.
2. The residual network `G_f` contains no augmenting paths from s to t.
3. `|f| = c(S, T)` for some cut (S, T) of G.

#### How to Find the Minimum Cut once Max Flow is Reached:
1. Run Ford-Fulkerson / Edmonds-Karp until no path exists from s to t in G_f.
2. Define set `S` = { vertices reachable from source s in the final residual graph G_f }.
3. Define set `T = V - S`.
4. (S, T) is the **Minimum Cut**.
5. The sum of original capacities of edges crossing from S to T exactly equals |f*|.

---

## 5. Maximum Bipartite Matching

### 5.1 Problem Definition
- A graph G = (V, E) is **bipartite** if its vertices can be partitioned into two disjoint sets L and R such that every edge in E connects a vertex in L to a vertex in R: `V = L ∪ R`, `L ∩ R = ∅`, and `E ⊆ L × R`.
- A **matching** M ⊆ E is a subset of edges such that no two edges share a common vertex (each vertex has degree at most 1 in M).
- The **Maximum Bipartite Matching (MBM)** problem is to find a matching M of maximum cardinality (largest number of matched pairs).
- Practical examples: Job applicants to available jobs, students to dorm rooms, medical residents to hospitals.

### 5.2 Reduction to Max Flow Network
We can solve MBM in polynomial time by reducing it to a Maximum Flow problem on a directed graph G' = (V', E'):

1. **Construct Vertices**:
   `V' = L ∪ R ∪ {s, t}` (add a supersource s and supersink t).
2. **Construct Edges and Capacities**:
   - Directed edge `(s, u)` for each `u ∈ L`, with capacity `c(s, u) = 1`.
   - Directed edge `(u, v)` for each original undirected edge `(u, v) ∈ E` where `u ∈ L, v ∈ R`, with capacity `c(u, v) = 1` (or ∞).
   - Directed edge `(v, t)` for each `v ∈ R`, with capacity `c(v, t) = 1`.

### 5.3 Correctness & The Integrality Theorem
- **Integrality Theorem**:
  If the capacities of all edges in a flow network are integers, then the maximum flow f produced by the Ford-Fulkerson method has the property that `f(u, v)` is an integer for all u, v ∈ V.
- Since all capacities in G' are 1, every edge has flow `f(u, v) ∈ {0, 1}`.
- Because capacity of `(s, u)` is 1, at most 1 unit leaves any `u ∈ L` (at most one matched edge).
- Because capacity of `(v, t)` is 1, at most 1 unit enters any `v ∈ R` (at most one matched edge).
- Therefore, the set of edges `M = { (u, v) ∈ E : f(u, v) = 1 }` is a valid matching in G.
- `|M| = |f*|`: The cardinality of the maximum matching equals the value of the maximum flow!

### 5.4 Time Complexity for MBM
- In G', |V'| = |V| + 2 and |E'| = |E| + |V|.
- The value of maximum flow |f*| is at most `min(|L|, |R|) ≤ |V|`.
- Using Ford-Fulkerson, each augmentation takes `O(E') = O(E)` time and there are at most `|V|` augmentations.
- **Overall Time Complexity**: `O(V · E)`.
- (Using Hopcroft-Karp algorithm, this can be solved in `O(E · √V)`).

---

## 6. Fully Worked Numerical Example (Ford-Fulkerson Step-by-Step)

### Problem:
Find the maximum flow from source `s` to sink `t` in the following network:
- Nodes: `s, A, B, C, D, t`
- Directed Edges and Capacities:
  - `s → A`: cap 10
  - `s → B`: cap 10
  - `A → B`: cap 2
  - `A → C`: cap 4
  - `A → D`: cap 8
  - `B → D`: cap 9
  - `C → t`: cap 10
  - `D → C`: cap 6
  - `D → t`: cap 10

### Step-by-Step Execution:

#### Initial State:
All edge flows `f = 0`. Net Flow `|f| = 0`.

#### Iteration 1:
- Choose augmenting path: `P1 = s → A → D → t`
- Residual capacities on P1:
  - `c_f(s, A) = 10`
  - `c_f(A, D) = 8`
  - `c_f(D, t) = 10`
- Bottleneck capacity: `c_f(P1) = min(10, 8, 10) = 8`.
- Augment flow by +8 along P1:
  - `f(s, A) = 8`
  - `f(A, D) = 8`  *(saturated!)*
  - `f(D, t) = 8`
- Total Flow `|f| = 8`.

#### Iteration 2:
- Choose augmenting path: `P2 = s → B → D → C → t`
- Residual capacities on P2:
  - `c_f(s, B) = 10`
  - `c_f(B, D) = 9`
  - `c_f(D, C) = 6`
  - `c_f(C, t) = 10`
- Bottleneck capacity: `c_f(P2) = min(10, 9, 6, 10) = 6`.
- Augment flow by +6 along P2:
  - `f(s, B) = 6`
  - `f(B, D) = 6`
  - `f(D, C) = 6`  *(saturated!)*
  - `f(C, t) = 6`
- Total Flow `|f| = 8 + 6 = 14`.

#### Iteration 3:
- Choose augmenting path: `P3 = s → A → C → t`
- Residual capacities on P3:
  - `c_f(s, A) = 10 - 8 = 2`
  - `c_f(A, C) = 4`
  - `c_f(C, t) = 10 - 6 = 4`
- Bottleneck capacity: `c_f(P3) = min(2, 4, 4) = 2`.
- Augment flow by +2 along P3:
  - `f(s, A) = 8 + 2 = 10`  *(saturated!)*
  - `f(A, C) = 2`
  - `f(C, t) = 6 + 2 = 8`
- Total Flow `|f| = 14 + 2 = 16`.

#### Iteration 4:
- Choose augmenting path: `P4 = s → B → D → t`
- Residual capacities on P4:
  - `c_f(s, B) = 10 - 6 = 4`
  - `c_f(B, D) = 9 - 6 = 3`
  - `c_f(D, t) = 10 - 8 = 2`
- Bottleneck capacity: `c_f(P4) = min(4, 3, 2) = 2`.
- Augment flow by +2 along P4:
  - `f(s, B) = 6 + 2 = 8`
  - `f(B, D) = 6 + 2 = 8`
  - `f(D, t) = 8 + 2 = 10`  *(saturated!)*
- Total Flow `|f| = 16 + 2 = 18`.

#### Iteration 5: Check for Augmenting Paths in Residual Graph G_f:
- From `s`, we can reach `B` (residual capacity `c_f(s, B) = 10 - 8 = 2`).
- From `B`, remaining capacity to `D` is `c_f(B, D) = 9 - 8 = 1`. So `D` is reachable.
- From `D`:
  - `D → t` is full (0 remaining).
  - `D → C` is full (0 remaining).
  - Backward edge to `A`: `c_f(D, A) = f(A, D) = 8 > 0`! So `A` is reachable from `D`.
- From `A`:
  - `A → C` has `c_f(A, C) = 4 - 2 = 2 > 0`. So `C` is reachable!
- From `C`:
  - `C → t` has `c_f(C, t) = 10 - 8 = 2 > 0`.
- **New Augmenting Path Found (using backward edge D → A!)**:
  `P5 = s → B → D → A → C → t`
  - `c_f(s, B) = 2`
  - `c_f(B, D) = 1`
  - `c_f(D, A) = 8`  *(backward edge!)*
  - `c_f(A, C) = 2`
  - `c_f(C, t) = 2`
- Bottleneck: `c_f(P5) = min(2, 1, 8, 2, 2) = 1`.
- Update flow:
  - `f(s, B) = 8 + 1 = 9`
  - `f(B, D) = 8 + 1 = 9`  *(saturated!)*
  - `f(A, D) = 8 - 1 = 7`  *(cancelled 1 unit of flow!)*
  - `f(A, C) = 2 + 1 = 3`
  - `f(C, t) = 8 + 1 = 9`
- Total Flow `|f| = 18 + 1 = 19`.

#### Iteration 6: Termination & Min-Cut Identification:
- In G_f, start BFS/DFS from `s`:
  - `c_f(s, A) = 10 - 10 = 0` (cannot reach A directly)
  - `c_f(s, B) = 10 - 9 = 1 > 0` (reach B)
  - From `B`: `c_f(B, D) = 9 - 9 = 0` (cannot reach D)
  - No other edges out of {s, B}.
- **Set of reachable nodes S**: `S = { s, B }`.
- **Set of non-reachable nodes T**: `T = { A, C, D, t }`.
- Sink `t ∉ S`. Therefore, no augmenting path exists! **Algorithm terminates.**

#### Verification of Max-Flow Min-Cut:
Edges crossing from `S` to `T`:
1. `(s, A)`: capacity = 10
2. `(B, D)`: capacity = 9
Capacity of Cut c(S, T):
```
c(S, T) = c(s, A) + c(B, D) = 10 + 9 = 19
```
Since `|f| = 19 = c(S, T)`, the **Maximum Flow = 19** and the **Minimum Cut Capacity = 19**.
Max-Flow Min-Cut Theorem is verified!

---

## 7. High-Yield Exam Comparison Tables

### Comparison 1: Ford-Fulkerson vs Edmonds-Karp

| Feature | Ford-Fulkerson Method | Edmonds-Karp Algorithm |
| :--- | :--- | :--- |
| **Path Search Strategy** | Any search (DFS, BFS, or arbitrary) | Strictly Breadth-First Search (BFS) |
| **Path Metric** | Any augmenting path | Shortest path by number of edges (hops) |
| **Time Complexity** | `O(E · |f*|)` (pseudo-polynomial) | `O(V · E²)` (strongly polynomial) |
| **Dependence on Capacities**| Depends heavily on magnitude of |f*| | Independent of capacity values |
| **Irrational Capacities** | May not terminate or converge to wrong value | Always terminates correctly |
| **Max Augmentations** | Up to `|f*|` | At most `O(V · E)` |

---

### Comparison 2: Forward Edges vs Backward Edges in Residual Graphs

| Property | Forward Edge `(u, v)` | Backward Edge `(v, u)` |
| :--- | :--- | :--- |
| **Condition to Exist** | Original edge has spare capacity: `f(u, v) < c(u, v)` | Original edge carries flow: `f(u, v) > 0` |
| **Residual Capacity** | `c_f(u, v) = c(u, v) - f(u, v)` | `c_f(v, u) = f(u, v)` |
| **Physical Meaning** | Pushing additional flow from u to v | Cancelling / redirecting flow previously sent from u to v |
| **Flow Update** | `f(u, v) ← f(u, v) + c_f(P)` | `f(u, v) ← f(u, v) - c_f(P)` |

---

## 8. Common Student Traps & How to Avoid Them

1. **Trap: Adding reverse edges in cut capacity**:
   - *Mistake*: When calculating `c(S, T)`, student adds edges directed from `T` to `S`.
   - *Fix*: Cut capacity counts **ONLY** edges directed from `S` into `T` (`u ∈ S, v ∈ T`). Edges from `T` to `S` contribute zero to capacity!
2. **Trap: Forgetting backward edges in residual graphs**:
   - *Mistake*: Searching for augmenting paths only along original edges with `c > f`.
   - *Fix*: If `f(u, v) > 0`, there is a backward edge `v → u` with capacity `f(u, v)`. Without backward edges, the algorithm cannot redirect flow and will get stuck in sub-optimal local maxima.
3. **Trap: Confusion between Ford-Fulkerson and Edmonds-Karp**:
   - *Mistake*: Calling Edmonds-Karp a separate algorithm from Ford-Fulkerson.
   - *Fix*: Edmonds-Karp IS the Ford-Fulkerson method implemented with BFS shortest augmenting paths.
4. **Trap: Forgetting the Integrality Theorem in Bipartite Matching**:
   - *Mistake*: Failing to explain why the flow in MBM doesn't split into fractions (e.g. 0.5 flow).
   - *Fix*: State the Integrality Theorem: Integer capacities guarantee an all-integer max flow. With unit capacities, flow is strictly 0 or 1, yielding a discrete matching.
