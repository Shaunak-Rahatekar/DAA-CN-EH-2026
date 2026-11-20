# DAA Unit III: One-Page Exam Cheat Sheet
**Course:** CS3212 (Design and Analysis of Algorithms) | **Unit III:** Dynamic Programming

> **Source Files Used:**
> - `DAA SYLLABUS.pdf` (Unit III)
> - `DAA Endsem Revision (CS3212).pdf` (pp. 2, 5)
> - `DAAOA_Question_Bank.docx` (Unit III)
> - `DAA_Question_Bank_Ans_Final.docx` (Unit III)

---

## 1. Dynamic Programming Master Recurrence Matrix

| Problem | Recurrence Equation | Time Complexity | Space Complexity | Decision Paradigm |
|---------|---------------------|:---------------:|:----------------:|:-----------------:|
| **Fibonacci** | `F(n) = F(n-1) + F(n-2)` | `O(n)` | `O(1)` (2 vars) | Linear Tabulation |
| **0/1 Knapsack** | `K[i,w] = max(K[i-1,w], v_i + K[i-1, w - w_i])` | `O(n · W)` | `O(n · W)` / `O(W)` | Include / Exclude item |
| **Matrix Chain (MCM)**| `m[i,j] = min { m[i,k] + m[k+1,j] + p_(i-1)·p_k·p_j }` | `O(n³)` | `O(n²)` | Split index `k` (`i ≤ k < j`) |
| **LCS** | `L[i,j] = L[i-1,j-1] + 1` (match), else `max(L[i-1,j], L[i,j-1])`| `O(m · n)` | `O(m · n)` | Diagonal / Left-Up trace |
| **Floyd-Warshall** | `D^k[i,j] = min( D^(k-1)[i,j], D^(k-1)[i,k] + D^(k-1)[k,j] )` | `Θ(V³)` | `O(V²)` | Vertex `k` as intermediate |
| **OBST** | `e[i,j] = min { e[i, r-1] + e[r+1, j] + w(i, j) }` | `O(n³)` | `O(n²)` | Root candidate `r` |
| **TSP (Held-Karp)**| `C(S, j) = min { C(S - {j}, i) + d[i, j] }` | `O(n² · 2ⁿ)` | `O(n · 2ⁿ)` | Subset bitmask DP |
| **DAG Shortest Path**| Relax edges in topological sorting order | `Θ(V + E)` | `O(V)` | Topological order |
| **Coin Change (Min)**| `c[v] = 1 + min { c[v - d_i] }` | `O(V · n)` | `O(V)` | Coin denomination |
| **LIS** | `L[i] = 1 + max { L[j] }` for `j < i, A[j] < A[i]` | `O(n²)` / `O(n log n)` | `O(n)` | Preceding smaller element |
| **Independent Set (Tree)**| `I(v) = max(1 + Sum I(grandchildren), Sum I(children))` | `O(n)` | `O(n)` | Root inclusion/exclusion |

---

## 2. Core Differences: DP vs Divide-and-Conquer vs Greedy

| Dimension | Dynamic Programming | Divide-and-Conquer | Greedy Strategy |
|-----------|---------------------|--------------------|-----------------|
| **Subproblem Relationship** | Heavily overlapping subproblems. | Disjoint, independent subproblems. | No overlapping subproblems. |
| **Decision Formulation** | Compares all possible choices at each step. | Solves all subproblems unconditionally. | Commits to single best local choice. |
| **Table Storage** | Stores solutions in array/hash table. | Does not store subproblem solutions. | No memory table needed. |
| **Optimality Guarantee** | Always guarantees global optimum. | Yes, by merging solutions. | Guarantees global optimum ONLY if greedy choice holds. |

---

## 3. High-Yield Exam Formulas & Trace Shortcuts

- **MCM Catalan Parenthesizations:** `C(n-1) = (2(n-1))! / ((n-1)! · n!) = Ω(4ⁿ / n^1.5)`.
- **0/1 Knapsack Traceback:**
  - If `K[i, w] == K[i-1, w]`: Item `i` was **NOT taken**. Move to `(i-1, w)`.
  - If `K[i, w] ≠ K[i-1, w]`: Item `i` **WAS taken**! Remaining weight = `w - w_i`. Move to `(i-1, w - w_i)`.
- **LCS Traceback:**
  - If `X[i] == Y[j]`: Character is in LCS! Output char, move diagonally `(i-1, j-1)`.
  - If `X[i] ≠ Y[j]`: Move to larger neighbor: up `(i-1, j)` or left `(i, j-1)`.
- **Floyd-Warshall Negative Cycle Check:**
  - If after phase `k = n`, any diagonal element `D[i, i] < 0`, the graph contains a **negative weight cycle**.

---

## 4. Common Exam Traps to Avoid
- **Trap 1:** Writing `O(n W)` as a polynomial time complexity for 0/1 Knapsack. *(It is **pseudo-polynomial** because `W` is a numeric value; in terms of input bits `b = log₂ W`, runtime is `O(n · 2ᵇ)`, which is exponential!)*
- **Trap 2:** MCM table dimensions: if matrices are `A1 (p0 × p1) ... An (p_(n-1) × pn)`, dimension array `p` has length `n + 1`. Table `m` is `(n+1) × (n+1)`, evaluated for chain lengths `L = 2` to `n`.
- **Trap 3:** Greedy for 0/1 Knapsack: Never use value/weight greedy ratio for 0/1 Knapsack. Greedy works **only** for Fractional Knapsack!
- **Trap 4:** Floyd-Warshall intermediate loops: loop over `k` MUST be the **outermost loop**, not innermost!
