# DAA: 60-Minute Last-Hour Exam Revision Checklist

**Course Code**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Timing**: Review this sheet in the final 60 minutes before entering the exam hall (10:00 AM – 11:00 AM).

---

## Part 1: 20 Crucial Facts to Commit to Memory

1. **Master Theorem Critical Cases**:
   - Case 1: `f(n) = O(n^(log_b a - ε))` ⟹ `T(n) = Θ(n^(log_b a))`.
   - Case 2: `f(n) = Θ(n^(log_b a) · log^k n)` ⟹ `T(n) = Θ(n^(log_b a) · log^(k+1) n)`.
   - Case 3: `f(n) = Ω(n^(log_b a + ε))` and regularity holds ⟹ `T(n) = Θ(f(n))`.
2. **Comparison Sort Lower Bound**: Every comparison sort requires `Ω(n log n)` comparisons in the worst case (proven by Decision Tree height `h ≥ log_2(n!) = Ω(n log n)`).
3. **Loop Invariant 3 Steps**: Initialization (true before loop), Maintenance (true before next iteration), Termination (gives meaningful proof of correctness upon termination).
4. **Closest Pair of Points**: Divide-and-conquer runs in `O(n log n)`. In the `2δ` strip, each point needs to be compared with at most **7** subsequent points sorted by y-coordinate.
5. **Karatsuba Integer Multiplication**: Uses **3 multiplications** instead of 4:
   `T(n) = 3T(n/2) + O(n)` ⟹ `O(n^(log_2 3)) = O(n^1.585)`.
6. **Median of Medians**: Divides into groups of **5**, finds medians, takes median-of-medians as pivot. Guarantees worst-case `O(n)` selection time (`T(n) ≤ T(n/5) + T(7n/10) + O(n)`).
7. **Disjoint Sets Union-Find**: Weighted Union (Union by Rank) + Collapsing Find (Path Compression) gives nearly linear time `O(m · α(n))` where `α(n)` is the inverse Ackermann function (`α(n) ≤ 4`).
8. **Dynamic Programming Two Hallmarks**: **Optimal Substructure** (optimal solution contains optimal subproblem solutions) and **Overlapping Subproblems** (same subproblems solved repeatedly).
9. **Matrix Chain Multiplication Recurrence**:
   `m[i, j] = min { m[i, k] + m[k+1, j] + p_(i-1) · p_k · p_j }` for `i ≤ k < j`. Table filled diagonally (`len = 2 to n`). Runtime `O(n³)`.
10. **Longest Common Subsequence (LCS)**:
    - If `X[i] == Y[j]`: `c[i, j] = c[i-1, j-1] + 1` (arrow `↖`).
    - Else: `c[i, j] = max(c[i-1, j] (↑), c[i, j-1] (←))`.
11. **0/1 Knapsack vs Fractional Knapsack**:
    - Fractional Knapsack: Solved greedily by sorting by value-to-weight ratio `v_i / w_i` in `O(n log n)`.
    - 0/1 Knapsack: Greedy FAILS! Solved by DP in `O(n · W)` pseudo-polynomial time or Branch & Bound.
12. **Floyd-Warshall All-Pairs Shortest Paths**:
    `D^(k)[i, j] = min( D^(k-1)[i, j], D^(k-1)[i, k] + D^(k-1)[k, j] )`. Outer loop `k = 1 to n`. Time `O(n³)`.
13. **Huffman Coding Property**: Prefix code (no codeword is a prefix of another). Built using a min-heap in `O(n log n)` time. Produces mathematically optimal prefix code.
14. **Prim's vs Kruskal's MST**:
    - Prim's: Grows a single connected tree from a start vertex. Best with Fibonacci heap `O(E + V log V)` for dense graphs.
    - Kruskal's: Adds globally minimum edge that doesn't create a cycle using Union-Find. Time `O(E log E) = O(E log V)`. Best for sparse graphs.
15. **Network Flow Axioms**: Capacity constraint `0 ≤ f(u, v) ≤ c(u, v)`, Skew symmetry `f(u, v) = -f(v, u)`, Flow conservation `Σ f(u, v) = 0` for `u ≠ s, t`.
16. **Residual Capacity**: Forward `c_f(u, v) = c(u, v) - f(u, v)`, Backward `c_f(v, u) = f(u, v)`.
17. **Max-Flow Min-Cut Theorem**: Max flow value = Capacity of Minimum Cut `c(S, T)`. Min-cut found by reachable vertices `S` from `s` in final residual graph `G_f`.
18. **Edmonds-Karp Complexity**: `O(V · E²)` because BFS ensures shortest distance `δ_f(s, v)` increases monotonically, each edge becomes critical at most `|V| / 2` times, so at most `O(V · E)` augmentations.
19. **Bipartite Matching Reduction**: Add supersource `s` (cap 1 to L) and supersink `t` (cap 1 from R). Original edges cap 1. Integrality theorem guarantees 0-1 flow ⟹ `|Max Matching| = |Max Flow|` in `O(V · E)`.
20. **P, NP, NP-Complete**:
    - `P`: Solvable in deterministic polynomial time.
    - `NP`: Verifiable in deterministic polynomial time.
    - `NP-Complete`: In NP and NP-Hard (Cook-Levin: SAT is NPC).
    - If any NPC problem has a poly-time algorithm ⟹ `P = NP`.

---

## Part 2: 5 Formulas & Diagrams to Redraw in the Exam

### 1. Master Theorem Decision Diagram:
```text
           Compare f(n) with n^(log_b a):
                        |
       +----------------+----------------+
       |                                 |
f(n) < n^(log_b a - ε)            f(n) = Θ(n^(log_b a) · log^k n)
(Polynomially smaller)                   |
       |                          T(n) = Θ(n^(log_b a) · log^(k+1) n)
T(n) = Θ(n^(log_b a))
```

### 2. Matrix Chain Recurrence & Table Diagonal Fill:
- Formula: `m[i, j] = min_{i ≤ k < j} { m[i, k] + m[k+1, j] + p_(i-1) · p_k · p_j }`.
- Fill order: Diagonals `d = 1` (zeros), `d = 2` (pairs), `d = 3` (triplets) up to `d = n`.

### 3. Residual Network Edge Rule:
- Forward Edge: `c_f(u, v) = c(u, v) - f(u, v)` (Solid arrow, remaining capacity).
- Backward Edge: `c_f(v, u) = f(u, v)` (Dotted arrow, cancelable flow).

### 4. Cut Capacity (Max-Flow Min-Cut):
- Formula: `c(S, T) = Σ c(u, v)` for `u ∈ S, v ∈ T` **only**.
- NEVER count edges pointing backwards from `T` to `S`!

### 5. 2-Approximation for Vertex Cover (Maximal Matching):
- Pick edge `(u, v)`, add both endpoints to `C`: `|C| = 2 · |M|`.
- Any optimal cover must pick at least 1 vertex per disjoint edge in `M`: `|C*| ≥ |M|`.
- Ratio: `|C| / |C*| ≤ (2 · |M|) / |M| = 2`.

---

## Part 3: 5 High-Penalty Traps & How to Avoid Them

1. **Trap 1: Adding reverse edges in Cut Capacity `c(S, T)`**:
   - *Mistake*: Summing all edges between S and T regardless of direction.
   - *Fix*: Count **ONLY** edges pointing from `S` into `T`. Reverse edges (`T → S`) contribute `0`.
2. **Trap 2: Saying NP stands for "Non-Polynomial"**:
   - *Mistake*: Writing that NP means exponential or non-polynomial.
   - *Fix*: NP stands for **Nondeterministic Polynomial**. Problems in NP are verifiable in polynomial time. Remember `P ⊆ NP`.
3. **Trap 3: Using Greedy on 0/1 Knapsack**:
   - *Mistake*: Sorting by `v/w` ratio and picking items for 0/1 Knapsack.
   - *Fix*: Greedy only works for *Fractional* Knapsack. For 0/1 Knapsack, use Dynamic Programming (`K[i, w]`).
4. **Trap 4: Forgetting backward edges in Ford-Fulkerson**:
   - *Mistake*: Only checking forward edges in residual graph.
   - *Fix*: If `f(u, v) > 0`, there is a backward edge `v → u` with residual capacity `f(u, v)`. Backward edges are essential to redirect suboptimal flow.
5. **Trap 5: Forgetting to prove BOTH directions in NP-Completeness**:
   - *Mistake*: Proving only that `x ∈ L' ⟹ f(x) ∈ L`.
   - *Fix*: You MUST prove both `(⇒)` If `x ∈ L'` then `f(x) ∈ L`, AND `(⇐)` If `f(x) ∈ L` then `x ∈ L'`.
