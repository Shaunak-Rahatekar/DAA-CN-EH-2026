# DAA Unit IV: Greedy and Backtracking Strategy

> **Sources Used:**
> - `D:\2026 EXAM\DAA\DAA SYLLABUS.pdf` (CS3212, Unit IV)
> - `D:\2026 EXAM\DAA\DAA Endsem Revision (CS3212).pdf` (Part A, Unit IV)
> - `D:\2026 EXAM\DAA\DAAOA_Question_Bank.docx` (Unit IV)
> - `D:\2026 EXAM\DAA\DAA_Question_Bank_Ans_Final.docx` (Unit IV Model Answers)
> - `D:\2026 EXAM\DAA\BOOKS\Introduction to Algorithms_Cormen_3rd Ed.pdf` (CLRS Chapters 16, 23, 24)
> - `D:\2026 EXAM\DAA\BOOKS\Computer Algorithms-Sahani.pdf` (Horowitz-Sahni Chapters 4, 7)

---

## Part 1: The Greedy Strategy

### 1.1 Fundamentals of the Greedy Paradigm
A **Greedy Algorithm** builds a solution step-by-step, always making the choice that looks best at the current moment (a locally optimal choice) in the hope that this will lead to a globally optimal solution.

#### Key Terminology:
- **Feasible Solution:** Any subset of candidate inputs that satisfies all problem constraints (e.g., in knapsack, any combination of items with total weight `≤ W`).
- **Optimal Solution:** A feasible solution that optimizes (maximizes or minimizes) the target objective function (e.g., the knapsack subset that yields the absolute maximum total profit).

#### Two Mandatory Properties for Greedy Optimality:
1. **Greedy-Choice Property:** A globally optimal solution can be arrived at by making a locally optimal (greedy) choice without reconsidering past choices.
2. **Optimal Substructure:** An optimal solution to the problem contains optimal solutions to its subproblems.

#### Proving Correctness of a Greedy Algorithm:
1. **Exchange Argument:** Show that any optimal solution that differs from the greedy solution can be gradually transformed into the greedy solution by exchanging elements without decreasing the objective value.
2. **Staying Ahead:** Prove by induction that at each step `k`, the greedy choice is at least as good as the choice made by any other algorithm.

---

### 1.2 Minimum Spanning Tree (MST)
- **Definition:** Given a connected, undirected graph `G = (V, E)` with edge weights, a **Spanning Tree** is an acyclic subgraph that connects all `|V|` vertices using exactly `|V| - 1` edges. A **Minimum Spanning Tree (MST)** is a spanning tree whose sum of edge weights is minimized.
- **The Cut Property (Foundation of MSTs):**
  For any cut `(S, V - S)` of the graph, if an edge `(u, v)` is the lightest (cheapest) edge crossing the cut, then this edge **must belong to some MST** of `G`.

#### 1. Kruskal’s Algorithm (Edge-Centric)
- **Strategy:**
  1. Sort all `|E|` edges in non-decreasing order of weight.
  2. Initialize an empty forest where each vertex is in its own disjoint set.
  3. Iterate through sorted edges: if adding edge `(u, v)` does NOT create a cycle (tested via `Find(u) ≠ Find(v)`), add it to the MST and merge sets via `Union(u, v)`.
  4. Stop when exactly `|V| - 1` edges are added.
- **Time Complexity:**
  - Sorting edges: `O(E log E) = O(E log V)`.
  - Disjoint Set operations: `O(E · α(V))`.
  - **Total Time Complexity:** **`O(E log E) = O(E log V)`**.
  - **Space Complexity:** `O(V + E)` for DSU arrays and edge list.
- **Best Suited For:** **Sparse graphs** where `E << V²`.

#### 2. Prim’s Algorithm (Vertex-Centric)
- **Strategy:**
  1. Start with an arbitrary root vertex `r`. Grow a single tree one vertex at a time.
  2. Maintain a Min-Priority Queue of vertices not yet in the tree, keyed by their minimum connection weight to any tree vertex.
  3. In each step, extract vertex `u` with minimum key (crossing cut `(S, V - S)`), add `u` to tree, and relax all edges `(u, v)` leaving `u`.
- **Time Complexity:**
  - Using Min-Heap (Binary Heap): **`O((V + E) log V) = O(E log V)`**.
  - Using Fibonacci Heap: **`O(E + V log V)`**.
  - Using Adjacency Matrix (no heap): **`O(V²)`** (ideal for dense graphs where `E ≈ V²`).
- **Best Suited For:** **Dense graphs** where `E ≈ V²`.

#### Comparison: Kruskal vs Prim

| Feature | Kruskal’s Algorithm | Prim’s Algorithm |
|---------|---------------------|------------------|
| **Core Idea** | Picks the globally cheapest edge that creates no cycle. | Grows a connected tree from a starting vertex by picking cheapest frontier edge. |
| **Data Structure** | Disjoint Set Union (DSU / Union-Find). | Min-Priority Queue (Binary Heap / Fibonacci Heap). |
| **Intermediate State** | A forest of disjoint trees that gradually merge. | Always maintains a single connected tree. |
| **Best Graph Type** | Sparse graphs (`E = O(V)`). | Dense graphs (`E = O(V²)`). |
| **Running Time** | `O(E log E) = O(E log V)` | `O(E log V)` (Heap) or `O(V²)` (Matrix). |

---

### 1.3 Dijkstra’s Single Source Shortest Path Algorithm
- **Problem:** Given a directed/undirected graph with non-negative edge weights (`w(u, v) ≥ 0`) and a source vertex `s`, find shortest paths from `s` to all other vertices.
- **Greedy Choice:** In each step, the unvisited vertex `u` with the smallest tentative distance `dist[u]` is finalized.
- **Why Non-Negative Weights are Required:**
  Dijkstra relies on the property that path weights grow monotonically. If negative edge weights exist, a path that looked longer could later become shorter via a negative edge, breaking the greedy finalization step. (For negative weights, Bellman-Ford must be used).
- **Time Complexity:**
  - With Binary Min-Heap: **`O((V + E) log V)`**.
  - With Adjacency Matrix: **`O(V²)`**.

---

### 1.4 Fractional Knapsack Problem
- **Problem:** Capacity `W`. Items can be divided into arbitrary continuous fractions `x_i ∈ [0, 1]`.
- **Greedy Rule:** Sort items in non-increasing order of **value-to-weight ratio** `r_i = v_i / w_i`. Take full items while they fit; take fractional part of the first item that exceeds remaining capacity.
- **Time Complexity:** Sorting takes **`O(n log n)`**; greedy packing takes `O(n)`. Total = `O(n log n)`.
- **Why Greedy Works Here:** Taking highest value per unit weight leaves maximum capacity for subsequent items.

---

### 1.5 Conflict-Free Activity Scheduling (Interval Scheduling)
- **Problem:** Given `n` activities with start times `s_i` and finish times `f_i`, select the maximum number of mutually non-overlapping activities.
- **Greedy Choice:** Always pick the activity that **finishes earliest** (`min f_i`) and is compatible with the previous activity.
- **Why Earliest Finish Time is Optimal:** Finishing as early as possible frees up the maximum remaining time resource for future activities.
- **Algorithm:**
  1. Sort activities by finish time `f_i` in ascending order (`O(n log n)`).
  2. Select the first activity `A1`.
  3. Iterate: select activity `k` if its start time `s_k ≥ f_last`.
- **Time Complexity:** **`O(n log n)`** due to sorting.

---

### 1.6 Job Sequencing with Deadlines
> **Source:** `DAAOA_Question_Bank.docx`, Unit IV, Q13(a) & `DAA_Question_Bank_Ans_Final.docx`

- **Problem:** Given `n` jobs where each job `i` takes 1 unit of time, has a deadline `d_i`, and yields profit `p_i` if completed on or before `d_i`. Find the job schedule that maximizes total profit.
- **Greedy Strategy:**
  1. Sort jobs in descending order of profit `p_i`.
  2. For each job, schedule it in the **latest possible empty time slot `t`** such that `1 ≤ t ≤ d_i`.
  3. If no empty slot is available before its deadline, discard the job.
- **Worked Exam Numerical (n = 5 Jobs):**
  - Jobs: `J1(d=2, p=20)`, `J2(d=2, p=15)`, `J3(d=1, p=10)`, `J4(d=3, p=5)`, `J5(d=3, p=1)`.
  - Max deadline = 3 → 3 time slots available: `Slot 1 [0-1]`, `Slot 2 [1-2]`, `Slot 3 [2-3]`.
  - Step 1: `J1 (p=20, d=2)`: Schedule in latest open slot `≤ 2` → **Slot 2**.
  - Step 2: `J2 (p=15, d=2)`: Slot 2 full. Latest open slot `≤ 2` is **Slot 1** → Schedule in Slot 1.
  - Step 3: `J3 (p=10, d=1)`: Slot 1 full. No slot `≤ 1` available → Rejected.
  - Step 4: `J4 (p=5, d=3)`: Latest open slot `≤ 3` is **Slot 3** → Schedule in Slot 3.
  - Step 5: `J5 (p=1, d=3)`: All slots [1, 2, 3] full → Rejected.
  - **Optimal Schedule:** `[J2, J1, J4]`.
  - **Maximum Profit:** `15 + 20 + 5 = 40`.
- **Time Complexity:** `O(n²)` with array slots; `O(n log n)` using Disjoint Set Union (DSU) for slot tracking.

---

### 1.7 Huffman Coding (Optimal Prefix-Free Codes)
> **Source:** `DAA Endsem Revision (CS3212).pdf`, Part C, Q14

- **Problem:** Given characters with frequencies, construct a binary code that minimizes expected encoded file size.
- **Properties:**
  - **Prefix-Free Code:** No codeword is a prefix of any other codeword (enables unambiguous instant decoding without delimiters).
  - Characters with higher frequencies get shorter codewords; characters with lower frequencies get longer codewords.
- **Greedy Tree Construction (Min-Heap):**
  1. Insert all characters as leaf nodes into a Min-Priority Queue keyed by frequency.
  2. While queue has more than 1 node:
     a. Extract two nodes with lowest frequencies: `x` and `y`.
     b. Create a new internal node `z` with frequency `freq[z] = freq[x] + freq[y]`, with left child `x` and right child `y`.
     c. Insert `z` back into Min-Heap.
  3. The remaining single node is the root of the Huffman Tree. Assign `0` to left edges and `1` to right edges.
- **Time Complexity:** Building heap: `O(n)`. Doing `n - 1` extractions and insertions: **`O(n log n)`**.

#### Worked Exam Example:
> **Frequencies:** `a:45, b:13, c:12, d:16, e:9, f:5` (Total characters = 100).

- **Step 1:** Lowest are `f:5` and `e:9`. Merge into `(ef):14` (children: `f=0, e=1`).
  - Heap: `c:12, b:13, (ef):14, d:16, a:45`.
- **Step 2:** Lowest are `c:12` and `b:13`. Merge into `(cb):25` (children: `c=0, b=1`).
  - Heap: `(ef):14, d:16, (cb):25, a:45`.
- **Step 3:** Lowest are `(ef):14` and `d:16`. Merge into `(efd):30` (children: `ef=0, d=1`).
  - Heap: `(cb):25, (efd):30, a:45`.
- **Step 4:** Lowest are `(cb):25` and `(efd):30`. Merge into `(cbefd):55` (children: `cb=0, efd=1`).
  - Heap: `a:45, (cbefd):55`.
- **Step 5:** Merge `a:45` and `(cbefd):55` into root `T:100` (children: `a=0, cbefd=1`).

**Resulting Huffman Codewords:**
- `a`: **`0`** (1 bit)
- `c`: `100` (3 bits)
- `b`: `101` (3 bits)
- `f`: `1100` (4 bits)
- `e`: `1101` (4 bits)
- `d`: `111` (3 bits)

**Average Bits Per Character:**
```
L_avg = (45 · 1 + 13 · 3 + 12 · 3 + 16 · 3 + 9 · 4 + 5 · 4) / 100
      = (45 + 39 + 36 + 48 + 36 + 20) / 100 = 224 / 100 = 2.24 bits/char
```
*(Fixed-length 3-bit code would require 300 bits; Huffman achieves 224 bits, saving 25.3%!).*

---

## Part 2: The Backtracking Strategy

### 2.1 Fundamentals of Backtracking
**Backtracking** is a systematic method for solving combinatorial constraint satisfaction problems by exploring a **State-Space Tree** using **Depth-First Search (DFS)**.

#### Core Mechanism:
1. Build a candidate solution vector `(x1, x2, ..., xk)` component-by-component.
2. At each node, evaluate a **Bounding Function (Feasibility / Pruning Constraint)**.
3. If the bounding function determines that the current partial solution cannot possibly be extended to a valid complete solution, **prune that entire subtree** and **backtrack** to the parent node to explore the next alternative.

---

### 2.2 The N-Queens Problem
- **Problem:** Place `n` non-attacking queens on an `n × n` chessboard.
- **Constraints:** No two queens can share the same row, same column, or same diagonal.
- **Representation:** Array `X[1 .. n]` where `X[i] = j` means queen in row `i` is placed in column `j`.
  - Row constraint: Guaranteed because each row gets exactly one queen `X[i]`.
  - Column constraint: `X[i] ≠ X[k]` for all `i ≠ k`.
  - Diagonal constraint: Two queens at `(i, X[i])` and `(k, X[k])` share a diagonal if and only if:
    ```
    |i - k| = |X[i] - X[k]|
    ```

#### 4-Queens Problem State-Space Tree
For $n = 4$:
- Queen 1 placed in Col 1: `X[1] = 1`.
  - Queen 2 in Col 3 (`X[2] = 3`):
    - Queen 3: Col 1 attacked (col), Col 2 attacked (diag with Q2), Col 3 attacked (col), Col 4 attacked (diag with Q2). **DEAD END (Prune!)**.
  - Queen 2 in Col 4 (`X[2] = 4`):
    - Queen 3 placed in Col 2 (`X[3] = 2`):
      - Queen 4: Col 1 (diag with Q3), Col 2 (col with Q3), Col 3 (diag with Q3), Col 4 (col with Q2). **DEAD END (Prune!)**.
  - Backtrack Queen 1 to Col 2: `X[1] = 2`!
    - Queen 2 placed in Col 4 (`X[2] = 4`).
    - Queen 3 placed in Col 1 (`X[3] = 1`).
    - Queen 4 placed in Col 3 (`X[4] = 3`).
    - All 4 queens valid! **First Valid Solution Found: `[2, 4, 1, 3]`**!
    - Symmetrical second solution: `[3, 1, 4, 2]`.

---

### 2.3 The Sum of Subsets Problem
> **Source:** `DAAOA_Question_Bank.docx`, Unit II, Q12(a)

- **Problem:** Given `n` positive integers `W = {w1, w2, ..., wn}` and a target sum `M`, find all subsets whose elements sum to exactly `M`.

#### Representation Approaches:
1. **Fixed-Tuple Sized Approach:** Solution vector `X = (x1, x2, ..., xn)` where `x_i ∈ {0, 1}` (1 = include item, 0 = exclude item). Binary state-space tree of depth `n` and `2ⁿ` leaves.
2. **Variable-Tuple Approach:** Solution vector `(x1, x2, ..., xk)` where `x_i` stores the index of the chosen element.

#### Bounding Functions (Pruning Criteria):
Sort weights in ascending order `w1 ≤ w2 ≤ ... ≤ wn`. Let current accumulated sum be `s`, and sum of all remaining elements be `r`:
1. **Right Pruning (Exceeds Target):** If `s + w_(i+1) > M`, prune all subsequent children!
2. **Left Pruning (Cannot Reach Target):** If `s + r < M`, even taking every remaining item cannot reach `M`. Prune!

#### Worked Exam Example:
> `n = 4`, weights `W = (11, 13, 24, 7)`, Target `M = 31`.

1. **Sort Weights:** `W = (7, 11, 13, 24)`. Total sum `r = 7 + 11 + 13 + 24 = 55`.
2. **Root (Level 0):** `s = 0, r = 55`.
   - Take `w1 = 7`: `s = 7, r = 48`.
     - Take `w2 = 11`: `s = 18, r = 37`.
       - Take `w3 = 13`: `s = 31`! **Target M = 31 reached!**
         **Solution 1: `{7, 11, 13}`** (or `x = (1, 1, 1, 0)`).
       - Exclude `w3 = 13`: `s = 18, r = 24`.
         - Take `w4 = 24`: `s = 18 + 24 = 42 > 31`. **PRUNE (Exceeds M)**.
     - Exclude `w2 = 11`: `s = 7, r = 37`.
       - Take `w3 = 13`: `s = 20, r = 24`.
         - Take `w4 = 24`: `s = 20 + 24 = 44 > 31`. **PRUNE**.
   - Exclude `w1 = 7`: `s = 0, r = 48`.
     - Take `w2 = 11`: `s = 11, r = 37`.
     - Exclude `w2 = 11`: `s = 0, r = 37`.
       - Take `w3 = 13`: `s = 13, r = 24`.
       - Exclude `w3 = 13`: `s = 0, r = 24`.
         - Take `w4 = 24`: `s = 24 < 31` and no elements left. **PRUNE**.
       - Take `w3 = 7` & `w4 = 24`: `7 + 24 = 31`!
         **Solution 2: `{7, 24}`** (or in original set: `{11, 13, 7}` and `{7, 24}`).

---

### 2.4 Graph Colouring Problem
> **Source:** `DAAOA_Question_Bank.docx`, Unit II, Q13

- **Problem:** Given an undirected graph `G = (V, E)` and `m` colors, assign a color to every vertex such that no two adjacent vertices have the same color.
- **Chromatic Number `χ(G)`:** The smallest integer `m` for which graph `G` can be properly colored.
- **Backtracking Algorithm:**
  1. Assign colors `1` to `m` to vertex `k`.
  2. Bounding Function: Check if any neighbor of `k` that is already colored shares color `x_k`.
  3. If safe, recurse to vertex `k + 1`. If no color in `1 .. m` is safe, backtrack to vertex `k - 1`.
- **Complexity:** Worst-case state-space tree has `mⁿ` nodes → **`O(mⁿ)`**.

---

### 2.5 Hamiltonian Cycle Problem
> **Source:** `DAAOA_Question_Bank.docx`, Unit II, Q11(b)

- **Problem:** Find a closed loop (cycle) in graph `G` that visits every vertex **exactly once** and returns to the start vertex.
- **Backtracking Procedure:**
  - Let path be `x[1 .. n]`. Start with `x[1] = 1`.
  - For position `k` (from `2` to `n`), candidate vertex `v` must satisfy:
    1. Edge `(x[k - 1], v)` must exist in graph.
    2. `v` must not be already visited (not present in `x[1 .. k-1]`).
    3. At position `k = n`, edge `(x[n], x[1])` must exist to close the cycle!
- **Complexity:** `O(n!)` in worst case.

---

### 2.6 Backtracking vs Branch-and-Bound (Exam Comparison)
> **Source:** `DAAOA_Question_Bank.docx`, Unit V, Q16(b)

| Feature | Backtracking | Branch and Bound (B&B) |
|---------|--------------|------------------------|
| **Tree Traversal** | **Depth-First Search (DFS)**. | **Breadth-First Search (BFS)** or **Best-First Search (LCBB)**. |
| **Data Structure** | Implicit call stack / recursion. | Explicit Queue (FIFO-BB) or Priority Queue (LCBB). |
| **Problem Type** | Decision problems and Constraint Satisfaction (N-Queens, Sudoku). | Pure Optimization problems (Min/Max: 0/1 Knapsack, TSP). |
| **Node Expansion** | Explores as deep as possible before backtracking. | Generates all children of current node before moving to next. |
| **Pruning Mechanism** | Bounding constraint (feasibility check). | Lower/Upper bound functions comparing with best objective so far. |
