# DAA Unit IV: One-Page Exam Cheat Sheet
**Course:** CS3212 (Design and Analysis of Algorithms) | **Unit IV:** Greedy & Backtracking Strategy

> **Source Files Used:**
> - `DAA SYLLABUS.pdf` (Unit IV)
> - `DAA Endsem Revision (CS3212).pdf` (pp. 3, 5)
> - `DAAOA_Question_Bank.docx` (Unit IV)
> - `DAA_Question_Bank_Ans_Final.docx` (Unit IV)

---

## 1. Greedy Algorithms Master Table

| Algorithm | Greedy Choice Criterion | Data Structure | Time Complexity | Optimality Proof |
|-----------|-------------------------|----------------|:---------------:|:----------------:|
| **Kruskal (MST)** | Globally cheapest edge that creates no cycle | Disjoint Set Union (DSU) | **`O(E log E) = O(E log V)`** | Cut Property / Exchange |
| **Prim (MST)** | Cheapest frontier edge connecting to tree | Min-Priority Queue | **`O(E log V)`** (Heap) / `O(V²)` | Cut Property / Staying ahead |
| **Dijkstra (SSSP)**| Unvisited vertex with smallest tentative distance | Min-Priority Queue | **`O((V + E) log V)`** | Non-negative edge property |
| **Fractional Knapsack**| Highest value-to-weight ratio (`v_i / w_i`) | Array sort | **`O(n log n)`** | Exchange argument |
| **Activity Scheduling**| Earliest finishing compatible activity (`min f_i`)| Array sort | **`O(n log n)`** | Staying ahead argument |
| **Job Sequencing** | Highest profit job in latest open slot `≤ d_i` | Array / DSU slots | **`O(n²)`** / `O(n log n)` | Priority slot packing |
| **Huffman Coding** | Merge two lowest-frequency nodes repeatedly | Min-Heap | **`O(n log n)`** | Optimal prefix-free tree |

---

## 2. Backtracking Algorithms Master Table

| Problem | Solution Vector Form | Bounding / Pruning Function | State-Space Tree Size | Worst-Case Time |
|---------|---------------------|-----------------------------|:---------------------:|:---------------:|
| **N-Queens** | `X[1 .. n]`, `X[i]` = column | No shared col: `X[i] ≠ X[k]`; No shared diag: `\|i - k\| ≠ \|X[i] - X[k]\|` | `nⁿ` nodes | `O(n!)` |
| **Sum of Subsets** | `(x1 .. xn)`, `x_i ∈ {0, 1}` | Prune if `sum + w_(i+1) > M` OR `sum + remaining < M` | `2ⁿ` leaves | `O(2ⁿ)` |
| **Graph Colouring**| `(c1 .. cn)`, `c_i ∈ {1 .. m}`| Prune if neighbor `v` has `color[v] == color[u]` | `mⁿ` nodes | `O(mⁿ)` |
| **Hamiltonian Cycle**| `(v1 .. vn)`, simple cycle | Edge `(v_(k-1), v_k)` exists, `v_k` unvisited, `(v_n, v1)` exists | `n!` permutations | `O(n!)` |
| **Sudoku** | `9 × 9` cell assignments | Digit 1-9 unique in row, unique in column, unique in 3×3 box | `9^k` leaves | `O(9^k)` |

---

## 3. Core High-Yield Exam Comparisons

### A. Kruskal vs Prim
- **Kruskal:** Edge-centric, uses DSU, intermediate state is a forest of multiple trees, best for **sparse graphs** (`E << V²`).
- **Prim:** Vertex-centric, uses Min-Heap, intermediate state is a single growing tree, best for **dense graphs** (`E ≈ V²`).

### B. Backtracking vs Branch and Bound
- **Backtracking:** Explores state-space via **Depth-First Search (DFS)** using recursion stack; primarily for **decision & constraint satisfaction problems** (N-Queens, Sudoku).
- **Branch and Bound:** Explores via **Breadth-First Search (FIFO-BB)** or **Best-First Search (LCBB)** using an explicit queue/priority queue; for **optimization problems** (Min/Max Knapsack, TSP).

---

## 4. Key Exam Formulas & Shortcuts

- **Huffman Coding Expected Length:**
  `L_avg = Sum(i=1 to n) [ p_i · length(codeword_i) ]`
- **4-Queens Solutions:**
  Two symmetrical solutions: `[2, 4, 1, 3]` and `[3, 1, 4, 2]`.
- **Job Sequencing Slots (n = 5):**
  Sort by profit descending. Place in slot `min(deadline, max_slot)` from right to left!
- **Dijkstra Failure Condition:**
  Fails if graph has **any negative edge weight** (because greedy finalization assumes distance never decreases).

---

## 5. Common Exam Traps to Avoid
- **Trap 1:** Forgetting to check diagonal condition in N-Queens: it is `|i - k| == |X[i] - X[k]|`. Both differences must be absolute values!
- **Trap 2:** Confusing Fractional Knapsack with 0/1 Knapsack: Greedy is optimal for **Fractional**, but fails for **0/1**!
- **Trap 3:** In Prim's algorithm, edge weights in the priority queue represent distance to the **MST tree**, NOT distance to the source (which is Dijkstra).
- **Trap 4:** Sum of Subsets: Always sort weights in **ascending order** first before drawing the state-space tree; otherwise pruning cannot safely discard all right subtrees.
