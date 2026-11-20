# DAA Unit V: Network Flows — One-Page Exam Cheat Sheet

**Course**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Scope**: Flow Networks, Ford-Fulkerson, Edmonds-Karp, Max-Flow Min-Cut, Bipartite Matching

---

### 1. Fundamental Properties of Network Flow
- **Capacity Constraint**: `0 ≤ f(u, v) ≤ c(u, v)` for all `u, v ∈ V`.
- **Skew Symmetry**: `f(u, v) = - f(v, u)` for all `u, v ∈ V`.
- **Flow Conservation**: For every intermediate vertex `u ∉ {s, t}`:
  `Σ f(u, v) = 0`  (Total Inflow = Total Outflow).
- **Net Flow Value**: `|f| = Σ f(s, v) = Σ f(v, t)`.

---

### 2. Residual Graph & Flow Augmentation
- **Residual Capacity**:
  - Forward edge `(u, v)`: `c_f(u, v) = c(u, v) - f(u, v)`
  - Backward edge `(v, u)`: `c_f(v, u) = f(u, v)`
- **Bottleneck Capacity**: `c_f(P) = min { c_f(u, v) : (u, v) ∈ P }`
- **Flow Update**:
  - Forward: `f(u, v) ← f(u, v) + c_f(P)`
  - Backward: `f(v, u) ← f(v, u) - c_f(P)`
- **New Flow**: `|f'| = |f| + c_f(P)`

---

### 3. Ford-Fulkerson vs Edmonds-Karp

| Feature | Ford-Fulkerson Method | Edmonds-Karp Algorithm |
| :--- | :--- | :--- |
| **Augmenting Path Choice** | Any valid path (DFS / BFS / arbitrary) | Shortest path by edge count (strictly BFS) |
| **Time Complexity** | `O(E · |f*|)` (pseudo-polynomial) | `O(V · E²)` (strongly polynomial) |
| **Max Augmentations** | Up to `|f*|` iterations | At most `O(V · E)` augmentations |
| **Irrational Capacities** | May not terminate or converge falsely | Always terminates correctly |

---

### 4. Cuts & Max-Flow Min-Cut Theorem
- **Cut (S, T)**: Partition of `V` into disjoint `S` and `T` such that `s ∈ S` and `t ∈ T`.
- **Net Flow across Cut**: `f(S, T) = Σ f(u, v) = |f|` (for `u ∈ S, v ∈ T`). Always equal to total flow!
- **Capacity of Cut**: `c(S, T) = Σ c(u, v)` for `u ∈ S, v ∈ T` **only** (ignore edges T → S!).
- **Weak Duality**: `|f| ≤ c(S, T)` for ANY flow f and ANY cut (S, T).
- **Max-Flow Min-Cut Theorem**: Max flow value = Min cut capacity.
- **Finding Min Cut**: `S` = all vertices reachable from `s` in final residual graph `G_f`; `T = V - S`.

---

### 5. Maximum Bipartite Matching (MBM)
- **Reduction**: Add source `s` and sink `t`.
  - Directed edge `s → u` for `u ∈ L`, capacity = 1.
  - Directed edge `u → v` for original edge `(u, v) ∈ E`, capacity = 1.
  - Directed edge `v → t` for `v ∈ R`, capacity = 1.
- **Integrality Theorem**: Integral capacities guarantee integral flow (`0` or `1`).
- **Result**: `|Matching M| = |Max Flow f*|`.
- **Complexity**: `O(V · E)` using Ford-Fulkerson.

---

### 6. Quick Memory Formulas
- Residual forward: `c_f = c - f`
- Residual backward: `c_f = f`
- Cut capacity: `c(S, T) = Σ c(u, v)` (`u ∈ S, v ∈ T`)
- Edmonds-Karp runtime: `O(V · E²)`
- Bipartite matching runtime: `O(V · E)`
