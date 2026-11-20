# DAA Unit IV: Model Answers & Marking Scheme (Greedy & Backtracking)

> **Sources Used:**
> - `DAA_Question_Bank_Ans_Final.docx` (Unit IV)
> - `DAA Endsem Revision (CS3212).pdf` (Part C, Q7, Q14, Q15, Q19)
> - `BOOKS\Introduction to Algorithms_Cormen_3rd Ed.pdf` (CLRS Chapters 16, 23)

---

## Solution Q1 [4 Marks]

### 1. Feasible vs Optimal Solution (2 Marks)
- **Feasible Solution:** Any candidate subset of the input that satisfies all specified problem constraints.
  - *Example:* In 0/1 Knapsack, any combination of items whose total weight does not exceed capacity `W`.
- **Optimal Solution:** A feasible solution that achieves the optimal (maximum or minimum) value of the objective function.
  - *Example:* The feasible subset of items that achieves the absolute maximum total profit.

### 2. Greedy-Choice & Optimal Substructure (2 Marks)
- **Greedy-Choice Property:** A global optimal solution can be assembled by making locally optimal (greedy) choices at each stage without reconsidering past decisions.
- **Optimal Substructure:** An optimal solution to the problem contains within it optimal solutions to its subproblems.

---

## Solution Q2 [4 Marks]

### Comparison Table (3 Marks)
| Feature | Prim’s Algorithm | Kruskal’s Algorithm |
|---------|------------------|---------------------|
| **Approach** | Grows a single tree outwards from a root vertex. | Merges disjoint trees by adding lightest cycle-free edge. |
| **Data Structure** | Min-Priority Queue (Binary Heap). | Disjoint Set Union (DSU / Union-Find). |
| **Intermediate State** | Always a single connected tree. | A forest of disjoint trees that gradually coalesce. |
| **Time Complexity** | `O(E log V)` (Heap) or `O(V²)` (Matrix). | `O(E log E) = O(E log V)`. |

### Graph Preference (1 Mark)
- **Dense Graphs (`E ≈ V²`):** **Prim’s Algorithm** with an adjacency matrix running in `O(V²)` is superior because edge-sorting in Kruskal takes `O(V² log V)`.
- **Sparse Graphs (`E = O(V)`):** **Kruskal’s Algorithm** running in `O(E log V)` is superior due to simple disjoint set operations.

---

## Solution Q3 [4 Marks]

### 1. Statement of Cut Property (2 Marks)
Let `G = (V, E)` be a connected, undirected graph with real edge weights. Let `(S, V - S)` be any partition (cut) of `V`. If edge `(u, v)` is the strictly minimum-weight edge crossing the cut `(S, V - S)`, then `(u, v)` **must belong to every Minimum Spanning Tree** of `G`.

### 2. Proof by Exchange Argument (2 Marks)
- Suppose there exists an MST `T` that does not contain the minimum crossing edge `e = (u, v)`.
- Since `T` is a spanning tree, adding edge `e` to `T` creates a unique cycle.
- This cycle must cross the cut `(S, V - S)` at least once more at some other edge `e'`.
- Since `e` is the minimum crossing edge:
  ```
  weight(e) < weight(e')
  ```
- If we delete `e'` from `T` and insert `e`, the new graph `T' = T - {e'} + {e}` remains a valid spanning tree.
- However:
  ```
  weight(T') = weight(T) - weight(e') + weight(e) < weight(T)
  ```
- This contradicts the assumption that `T` was an MST! Therefore, `e` must belong to the MST.

---

## Solution Q4 [4 Marks]

### 1. Greedy Rule for Activity Selection (1.5 Marks)
Sort all activities by **finish time `f_i` in non-decreasing order**.
1. Select the first activity `A1` (which finishes earliest).
2. For each subsequent activity `k`, select it if its start time `s_k ≥ f_last`. Otherwise, discard it.

### 2. Optimality Proof (Staying Ahead) (2.5 Marks)
- Let greedy solution be `G = (g1, g2, ..., gk)` and an arbitrary optimal solution be `O = (o1, o2, ..., om)`.
- We prove by induction that for all `r ≤ k`:
  ```
  finish(gr) ≤ finish(or)
  ```
- *Base case (r = 1):* Since greedy picks the activity with the minimum finish time across all activities, `finish(g1) ≤ finish(o1)`.
- *Inductive step:* Assume `finish(g_(r-1)) ≤ finish(o_(r-1))`. Since `o_r` is compatible with `o_(r-1)`, `start(o_r) ≥ finish(o_(r-1)) ≥ finish(g_(r-1))`. Thus, `o_r` is also compatible with `g_(r-1)`. Because greedy chooses the earliest finisher among all compatible activities, `finish(g_r) ≤ finish(o_r)`.
- *Conclusion:* Greedy leaves at least as much time remaining as any optimal solution. Thus, `k ≥ m`, proving greedy finds the maximum number of activities.

---

## Solution Q5 [4 Marks]

### 1. Mathematical Diagonal Condition (2 Marks)
Let queen 1 be at `(row = i, col = X[i])` and queen 2 be at `(row = k, col = X[k])`.
Two queens lie on the same diagonal if and only if the absolute difference between their row indices equals the absolute difference between their column indices:
```
|i - k| = |X[i] - X[k]|
```
Equivalent to:
`X[i] - X[k] = i - k`  OR  `X[i] - X[k] = -(i - k)`.

### 2. Example on 4×4 Board (2 Marks)
- Queen 1 placed at `Row 1, Col 1` → `(1, 1)`.
- Queen 2 placed at `Row 2, Col 2` → `(2, 2)`.
  - `|1 - 2| = 1` and `|1 - 2| = 1`. Equal! **Attacking on major diagonal!**
- Queen 3 placed at `Row 3, Col 1` → `|1 - 3| = 2`, `|1 - 1| = 0`. Different, but same column (blocked by column constraint).
- Queen 2 placed at `Row 2, Col 3` → `|1 - 2| = 1`, `|1 - 3| = 2`. Safe!

---

## Solution Q6 [4 Marks]

| Feature | Backtracking | Branch and Bound (B&B) |
|---------|--------------|------------------------|
| **Tree Traversal** | **Depth-First Search (DFS)**. | **Breadth-First Search (BFS)** or **Best-First Search (LCBB)**. |
| **Data Structure** | Call stack / recursion. | Explicit Queue (FIFO-BB) or Priority Queue (LCBB). |
| **Target Problem Type** | Decision problems & Constraint satisfaction (N-Queens, Sudoku, Hamiltonian). | Optimization problems (0/1 Knapsack, TSP). |
| **Pruning Condition** | Bounding constraint (feasibility check). | Compares node bound with best global solution found so far. |

---

## Solution Q7 [10 Marks]

### Part a: Huffman Tree Construction (6 Marks)
- Frequencies: `a:45, b:13, c:12, d:16, e:9, f:5`.

**Step-by-Step Min-Heap Merges:**
1. Extract `f:5` and `e:9`. Create node `(ef):14`. Left = `f(0)`, Right = `e(1)`.
   - Remaining: `c:12, b:13, (ef):14, d:16, a:45`.
2. Extract `c:12` and `b:13`. Create node `(cb):25`. Left = `c(0)`, Right = `b(1)`.
   - Remaining: `(ef):14, d:16, (cb):25, a:45`.
3. Extract `(ef):14` and `d:16`. Create node `(efd):30`. Left = `ef(0)`, Right = `d(1)`.
   - Remaining: `(cb):25, (efd):30, a:45`.
4. Extract `(cb):25` and `(efd):30`. Create node `(cbefd):55`. Left = `cb(0)`, Right = `efd(1)`.
   - Remaining: `a:45, (cbefd):55`.
5. Extract `a:45` and `(cbefd):55`. Create Root `T:100`. Left = `a(0)`, Right = `cbefd(1)`.

**Codewords Generated:**
- `a`: **`0`** (1 bit)
- `c`: `100` (3 bits)
- `b`: `101` (3 bits)
- `d`: `111` (3 bits)
- `f`: `1100` (4 bits)
- `e`: `1101` (4 bits)

### Part b: Average Length & Bit Savings (4 Marks)
```
L_avg = Sum(freq · length) / Total_chars
      = (45 · 1 + 13 · 3 + 12 · 3 + 16 · 3 + 9 · 4 + 5 · 4) / 100
      = (45 + 39 + 36 + 48 + 36 + 20) / 100
      = 224 / 100 = 2.24 bits/character
```
- **Total Bits with Huffman:** `224` bits.
- **Fixed-Length Encoding:** 6 characters require `⌈log₂ 6⌉ = 3` bits each.
  `Total = 100 × 3 = 300` bits.
- **Total Bits Saved:** `300 - 224 = 76` bits (**25.3% reduction**).

---

## Solution Q8 [10 Marks]

### Part a: 4-Queens State-Space Tree (6 Marks)
Let `X[row] = col`:
1. **Try `X[1] = 1`:**
   - Row 2:
     - `X[2] = 1` (Col conflict).
     - `X[2] = 2` (Diagonal conflict with Q1: `|2-1| = |2-1|`).
     - `X[2] = 3` (Safe).
       - Row 3:
         - `X[3] = 1` (Col conflict with Q1).
         - `X[3] = 2` (Diag conflict with Q2: `|3-2| = |2-3|`).
         - `X[3] = 3` (Col conflict with Q2).
         - `X[3] = 4` (Diag conflict with Q2: `|3-2| = |4-3|`).
         - **DEAD END. Backtrack!**
     - `X[2] = 4` (Safe).
       - Row 3: `X[3] = 2` (Safe).
         - Row 4: All columns 1, 2, 3, 4 are attacked. **DEAD END. Backtrack!**
2. **Backtrack to Row 1: Try `X[1] = 2`:**
   - Row 2: `X[2] = 4` (Safe: `|2-1|=1 ≠ |4-2|=2`).
   - Row 3: `X[3] = 1` (Safe: col 1 free; diags free).
   - Row 4: `X[4] = 3` (Safe: col 3 free; diags free).
   - **FIRST VALID SOLUTION FOUND:** **`[2, 4, 1, 3]`**!
   *(Symmetrical solution: `[3, 1, 4, 2]`)*.

### Part b: General N-Queens Algorithm & Complexity (4 Marks)
```text
NQueens(row, n):
  for col = 1 to n:
    if isSafe(row, col):
      X[row] = col
      if row == n:
        print(X[1 .. n])
      else:
        NQueens(row + 1, n)

isSafe(row, col):
  for i = 1 to row - 1:
    if X[i] == col or abs(i - row) == abs(X[i] - col):
      return False
  return True
```
- **Worst-Case Time:** Upper bounded by `O(n!)` (pruning avoids full `nⁿ` tree).
- **Space Complexity:** `O(n)` auxiliary array and recursion stack.

---

## Solution Q9 [10 Marks]

### Part a: Sum of Subsets State-Space Tree (7 Marks)
`W = (11, 13, 24, 7)`, `M = 31`.
- **Step 1: Sort Weights Ascending:** `W = (w1=7, w2=11, w3=13, w4=24)`. Total sum `r = 55`.

**State-Space Tree Exploration (s = current sum, r = remaining sum):**
- **Node 1 (Root):** `s = 0, r = 55`.
  - **Include w1 = 7:** `s = 7, r = 48`.
    - **Include w2 = 11:** `s = 18, r = 37`.
      - **Include w3 = 13:** `s = 18 + 13 = 31`! **Target reached!**
        **Solution 1: `{7, 11, 13}`** (Indices 1, 2, 3; or `x = (1, 1, 1, 0)`).
      - **Exclude w3 = 13:** `s = 18, r = 24`.
        - **Include w4 = 24:** `s = 18 + 24 = 42 > 31`. **PRUNE (Exceeds M)**.
    - **Exclude w2 = 11:** `s = 7, r = 37`.
      - **Include w3 = 13:** `s = 20, r = 24`.
        - **Include w4 = 24:** `s = 20 + 24 = 44 > 31`. **PRUNE**.
      - **Exclude w3 = 13:** `s = 7, r = 24`.
        - **Include w4 = 24:** `s = 7 + 24 = 31`! **Target reached!**
          **Solution 2: `{7, 24}`** (Indices 1, 4; or `x = (1, 0, 0, 1)`).
  - **Exclude w1 = 7:** `s = 0, r = 48`.
    - **Include w2 = 11:** `s = 11, r = 37`.
      - **Include w3 = 13:** `s = 24, r = 24`.
        - **Include w4 = 24:** `s = 24 + 24 = 48 > 31`. **PRUNE**.
      - **Exclude w3 = 13:** `s = 11, r = 24`.
        - `s + w4 = 11 + 24 = 35 > 31`. **PRUNE**.
    - **Exclude w2 = 11:** `s = 0, r = 37`.
      - `s + r = 13 + 24 = 37`.
      - **Include w3 = 13:** `s = 13, r = 24`.
        - `s + w4 = 13 + 24 = 37 > 31`. **PRUNE**.
      - **Exclude w3 = 13:** `s = 0, r = 24 < 31`. **PRUNE (Cannot reach M)**.

**Valid Solutions Found:**
1. `{7, 11, 13}` (Sum = 31)
2. `{7, 24}` (Sum = 31)

### Part b: Bounding Functions (3 Marks)
1. **Right Bounding Function (Feasibility):**
   ```
   s + w_(i+1) ≤ M
   ```
   If current sum plus next item exceeds `M`, prune all subsequent children.
2. **Left Bounding Function (Reachability):**
   ```
   s + r ≥ M
   ```
   If current sum plus all remaining items is strictly less than `M`, prune.

---

## Solution Q10 [8 Marks]

### Problem: Job Sequencing with Deadlines
`J1(d=2, p=20), J2(d=2, p=15), J3(d=1, p=10), J4(d=3, p=5), J5(d=3, p=1)`.

### Step-by-Step Slot Allocation (6 Marks)
Jobs are already sorted in descending order of profit.
Maximum deadline `D_max = 3`. Set up 3 slots: `Slot 1 [0-1]`, `Slot 2 [1-2]`, `Slot 3 [2-3]`.

1. **Job J1 (Profit = 20, Deadline = 2):**
   - Check slot 2: Empty.
   - Schedule **J1 in Slot 2**. Slots: `[Empty, J1, Empty]`. Profit = 20.
2. **Job J2 (Profit = 15, Deadline = 2):**
   - Check slot 2: Occupied by J1.
   - Check next open slot `≤ 2`: Slot 1 is empty.
   - Schedule **J2 in Slot 1**. Slots: `[J2, J1, Empty]`. Profit = `20 + 15 = 35`.
3. **Job J3 (Profit = 10, Deadline = 1):**
   - Check slot 1: Occupied by J2.
   - No earlier open slot available (`d=1`). **Reject J3**.
4. **Job J4 (Profit = 5, Deadline = 3):**
   - Check slot 3: Empty.
   - Schedule **J4 in Slot 3**. Slots: `[J2, J1, J4]`. Profit = `35 + 5 = 40`.
5. **Job J5 (Profit = 1, Deadline = 3):**
   - All slots 1, 2, 3 are occupied. **Reject J5**.

### Final Schedule & Maximum Profit (2 Marks)
- **Optimal Scheduled Sequence:** **`[J2, J1, J4]`** (or set of jobs `{J1, J2, J4}`).
- **Maximum Profit:** `15 + 20 + 5 =` **`40`**.

---

## Solution Q11 [8 Marks]

### Part a: Dijkstra's Algorithm (5 Marks)
- **Algorithm (3 Marks):**
  1. Set `dist[source] = 0` and all other `dist[v] = ∞`.
  2. Insert all vertices into Min-Priority Queue.
  3. While queue is not empty:
     a. Extract vertex `u` with minimum `dist[u]`.
     b. For each neighbor `v` of `u`, relax edge `(u, v)`:
        ```
        if dist[v] > dist[u] + weight(u, v):
            dist[v] = dist[u] + weight(u, v)
        ```
- **Why Non-Negative Weights are Required (2 Marks):**
  Dijkstra marks a vertex as finalized once extracted from the heap, assuming no shorter path to that vertex can exist. If negative weights exist, a longer positive path could later branch into a negative edge, invalidating the finalized distance.

### Part b: Graph Colouring & Chromatic Number (3 Marks)
- **Chromatic Number `χ(G)`:** The minimum number of colors required to color the vertices of `G` such that no two adjacent vertices share the same color.
- **State-Space Tree for `m = 3` colors, `n = 4` vertices:**
  - Root tests Vertex 1 with colors `{1, 2, 3}`.
  - Level 1 tests Vertex 2 with safe colors not sharing an edge with Vertex 1.
  - Branches that violate neighbor color constraints are immediately pruned.
  - Worst-case tree size = `mⁿ = 3⁴ = 81` leaves.
