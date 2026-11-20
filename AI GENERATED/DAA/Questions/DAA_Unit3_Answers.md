# DAA Unit III: Model Answers & Marking Scheme (Dynamic Programming)

> **Sources Used:**
> - `DAA_Question_Bank_Ans_Final.docx` (Unit III)
> - `DAA Endsem Revision (CS3212).pdf` (Part C, Q6, Q7, Q12, Q13, Q17)
> - `BOOKS\Introduction to Algorithms_Cormen_3rd Ed.pdf` (CLRS Chapters 15, 25)

---

## Solution Q1 [4 Marks]

### 1. Principle of Optimality (2 Marks)
The **Principle of Optimality** (formulated by Richard Bellman) states that an optimal sequence of decisions has the property that whatever the initial state and initial decisions are, the remaining decisions must constitute an optimal sequence of decisions with regard to the state resulting from the first decision.
- *In short:* An optimal solution to any instance of the problem is composed of optimal solutions to its subproblems.

### 2. Two Essential Preconditions for DP (2 Marks — 1 Mark each)
1. **Optimal Substructure:** The global optimal solution can be constructed efficiently from the optimal solutions of its subproblems.
   - *Example:* In Shortest Paths, if the shortest path from `A` to `C` passes through `B`, then the subpath from `A` to `B` must be the shortest path between `A` and `B`.
2. **Overlapping Subproblems:** A recursive solution generates the same subproblems repeatedly, allowing a table to store and reuse results.
   - *Example:* In Matrix Chain Multiplication, the subchain `(A2 · A3)` is needed repeatedly when evaluating both `((A1 · A2 · A3) · A4)` and `(A1 · (A2 · A3 · A4))`.

---

## Solution Q2 [4 Marks]

### Part 1: Top-Down (Memoization) vs Bottom-Up (Tabulation) (2 Marks)
| Criterion | Top-Down (Memoization) | Bottom-Up (Tabulation) |
|-----------|------------------------|------------------------|
| **Direction** | Starts from original problem; recurses downwards and caches subproblem outputs in an array or hash map. | Starts from base cases; fills table iteratively from smallest subproblems to largest. |
| **Call Stack** | Incurs recursion call stack overhead; risk of recursion limit errors. | Uses simple loops; zero recursion overhead. |
| **Subproblem Evaluation** | Computes only the subproblems actually needed. | Computes all subproblems in table order. |

### Part 2: Dynamic Programming vs Greedy Method (2 Marks)
| Criterion | Dynamic Programming | Greedy Method |
|-----------|---------------------|---------------|
| **Decision Formulation** | Considers all choices at each step and picks the one yielding the global minimum/maximum. | Makes a single locally optimal choice at each step without considering future effects. |
| **Backtracking** | Memorizes all paths; implicitly explores and weighs alternatives. | Never reconsiders or re-evaluates a decision once committed. |
| **Optimality Guarantee** | Always guarantees optimal solution if optimal substructure exists. | Guarantees optimum ONLY if the greedy-choice property holds. |

---

## Solution Q3 [4 Marks]

### 1. Why Greedy Works for Fractional Knapsack (1.5 Marks)
In the Fractional Knapsack problem, items can be broken into arbitrary fractions. The greedy choice of sorting items by **value-to-weight ratio (`v_i / w_i`)** and packing as much of the highest-ratio item as possible is provably optimal because any replacement of a higher-ratio unit with a lower-ratio unit strictly decreases total value.

### 2. Why Greedy Fails for 0/1 Knapsack (1.5 Marks)
In 0/1 Knapsack, items are indivisible (take all or none). Picking the item with the highest ratio may consume knapsack capacity that could have otherwise accommodated multiple items of slightly lower ratio whose combined value exceeds the single item.

### 3. Concrete Counterexample (1 Mark)
- Knapsack Capacity: `W = 50`.
- Items:
  - Item 1: `w1 = 10, v1 = 60` → Ratio = `6.0`
  - Item 2: `w2 = 20, v2 = 100` → Ratio = `5.0`
  - Item 3: `w3 = 30, v3 = 120` → Ratio = `4.0`
- **Greedy Strategy:**
  - Picks Item 1 (`w = 10, v = 60`). Remaining capacity = `40`.
  - Next picks Item 2 (`w = 20, v = 100`). Remaining capacity = `20`.
  - Item 3 (`w = 30`) does NOT fit!
  - **Greedy Total Value:** `60 + 100 = 160` (Weight = 30, 20 capacity wasted).
- **Optimal DP Strategy:**
  - Pick Item 2 and Item 3: Total weight = `20 + 30 = 50`, **Total Value = `100 + 120 = 220`**!
  - `220 > 160`, proving greedy fails.

---

## Solution Q4 [4 Marks]

### 1. Definition of Pseudo-Polynomial Time (2 Marks)
An algorithm runs in **pseudo-polynomial time** if its time complexity is bounded by a polynomial in the **numeric value** of the input data, rather than the length (number of bits) of the input.

### 2. Mathematical Distinction in 0/1 Knapsack (2 Marks)
- The input to the 0/1 Knapsack problem consists of `n` items and a numeric integer capacity `W`.
- In binary representation, the number of bits required to store the number `W` is:
  ```
  b = ⌈log₂ W⌉
  ```
- Therefore, the numeric value is exponential in terms of its bit length:
  ```
  W = 2ᵇ
  ```
- Expressed in terms of input size (bits):
  ```
  T(n) = O(n · W) = O(n · 2ᵇ)
  ```
- Since the runtime is **exponential in the number of bits `b`**, it is not a true polynomial time algorithm. It is polynomial ONLY if `W` is bounded by a small polynomial function of `n`.

---

## Solution Q5 [4 Marks]

### 1. Recurrence Relation for LCS (2 Marks)
Let `L[i, j]` be the length of the longest common subsequence of prefixes `X[1 .. i]` and `Y[1 .. j]`:
```
L[i, j] = 0   if i == 0 or j == 0

L[i, j] = L[i - 1, j - 1] + 1   if X[i] == Y[j]

L[i, j] = max( L[i - 1, j],  L[i, j - 1] )   if X[i] ≠ Y[j]
```

### 2. Solution for Given Sequences (2 Marks)
- Given: `X = "ABCBDAB"` and `Y = "BDCABA"`.
- **Length of LCS:** **4**.
- **Two Valid Longest Common Subsequences:**
  1. `"BCBA"`
  2. `"BDAB"` (or `"BCAB"`)

---

## Solution Q6 [4 Marks]

### 1. Floyd-Warshall Recurrence Relation (1.5 Marks)
```
D^k[i, j] = min( D^(k-1)[i, j],   D^(k-1)[i, k] + D^(k-1)[k, j] )   for k = 1 to n
```
Base case: `D^0` is the direct weight adjacency matrix `W` (with `D^0[i, i] = 0` and non-edges `= ∞`).

### 2. Time Complexity (1 Mark)
Three nested loops each iterating `1` to `V`:
```
Time Complexity = Θ(V³)
Space Complexity = O(V²)
```

### 3. Detection of Negative-Weight Cycles (1.5 Marks)
- In a valid graph without negative cycles, the shortest distance from any vertex to itself is always `0` (`D[i, i] = 0`).
- If a negative-weight cycle is reachable from vertex `i`, traversing that cycle repeatedly strictly decreases the path cost from `i` to `i` below 0.
- **Cycle Rule:** If after completing all `n` phases, **any diagonal entry `D^n[i, i] < 0`**, the graph contains at least one **negative weight cycle**.

---

## Solution Q7 [10 Marks]

### Part a: Recurrence Formulation (3 Marks)
Dimensions array: `p = [3, 2, 4, 2, 5]` (`n = 4` matrices).
```
m[i, j] = 0   if i == j

m[i, j] = min (i ≤ k < j) { m[i, k] + m[k + 1, j] + p_(i-1) · p_k · p_j }   for i < j
```

### Part b: Table Filling (5 Marks)
1. **Chain Length L = 1:**
   `m[1,1] = m[2,2] = m[3,3] = m[4,4] = 0`.
2. **Chain Length L = 2:**
   - `m[1, 2] = m[1,1] + m[2,2] + p0·p1·p2 = 0 + 0 + (3 · 2 · 4) = 24`, `s[1,2] = 1`.
   - `m[2, 3] = m[2,2] + m[3,3] + p1·p2·p3 = 0 + 0 + (2 · 4 · 2) = 16`, `s[2,3] = 2`.
   - `m[3, 4] = m[3,3] + m[4,4] + p2·p3·p4 = 0 + 0 + (4 · 2 · 5) = 40`, `s[3,4] = 3`.
3. **Chain Length L = 3:**
   - `m[1, 3]`:
     - `k=1: m[1,1] + m[2,3] + p0·p1·p3 = 0 + 16 + (3 · 2 · 2) = 28`
     - `k=2: m[1,2] + m[3,3] + p0·p2·p3 = 24 + 0 + (3 · 4 · 2) = 48`
     - Minimum = **28** at `k = 1`, `s[1,3] = 1`.
   - `m[2, 4]`:
     - `k=2: m[2,2] + m[3,4] + p1·p2·p4 = 0 + 40 + (2 · 4 · 5) = 80`
     - `k=3: m[2,3] + m[4,4] + p1·p3·p4 = 16 + 0 + (2 · 2 · 5) = 36`
     - Minimum = **36** at `k = 3`, `s[2,4] = 3`.
4. **Chain Length L = 4 (Full Chain m[1, 4]):**
   - `k=1: m[1,1] + m[2,4] + p0·p1·p4 = 0 + 36 + (3 · 2 · 5) = 66`
   - `k=2: m[1,2] + m[3,4] + p0·p2·p4 = 24 + 40 + (3 · 4 · 5) = 124`
   - `k=3: m[1,3] + m[4,4] + p0·p3·p4 = 28 + 0 + (3 · 2 · 5) = 58`
   - Minimum = **58** at `k = 3`, `s[1,4] = 3`.

**Final `m` Table:**
```
       1     2     3     4
1  |   0    24    28    58
2  |   -     0    16    36
3  |   -     -     0    40
4  |   -     -     -     0
```

### Part c: Parenthesization & Result (2 Marks)
- Minimum scalar multiplications = **`58`**.
- Split `s[1, 4] = 3` splits into `(A1 A2 A3) · (A4)`.
- Split `s[1, 3] = 1` splits into `(A1) · (A2 A3)`.
- **Optimal Parenthesization:** **`((A1 · (A2 · A3)) · A4)`**.

---

## Solution Q8 [10 Marks]

### Part a: 0/1 Knapsack Table Construction (6 Marks)
- Recurrence:
  `K[i, w] = max(K[i-1, w], v_i + K[i-1, w - w_i])` if `w_i ≤ w`, else `K[i-1, w]`.
- Items: `(w1=1, v1=1)`, `(w2=3, v2=4)`, `(w3=4, v3=5)`, `(w4=5, v4=7)`. Capacity `W = 7`.

**Tabulation Table K[i, w]:**

| Item `i` | `(w_i, v_i)` | w=0 | w=1 | w=2 | w=3 | w=4 | w=5 | w=6 | w=7 |
|:--------:|:------------:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **0** | Base | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| **1** | (1, 1) | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| **2** | (3, 4) | 0 | 1 | 1 | 4 | 5 | 5 | 5 | 5 |
| **3** | (4, 5) | 0 | 1 | 1 | 4 | 5 | 6 | 6 | 9 |
| **4** | (5, 7) | 0 | 1 | 1 | 4 | 5 | 7 | 8 | **9** |

*Maximum Value in Knapsack = `K[4, 7] = 9`.*

### Part b: Traceback & Item Selection (4 Marks)
- Start at `(i = 4, w = 7)`:
  - `K[4, 7] (9) == K[3, 7] (9)`: Value did not change! Item 4 was **NOT taken**. Move to `(i = 3, w = 7)`.
  - `K[3, 7] (9) ≠ K[2, 7] (5)`: Item 3 **WAS taken**!
    - Subtract weight: `w = 7 - w3 = 7 - 4 = 3`.
    - Move to `(i = 2, w = 3)`.
  - `K[2, 3] (4) ≠ K[1, 3] (1)`: Item 2 **WAS taken**!
    - Subtract weight: `w = 3 - w2 = 3 - 3 = 0`.
    - Move to `(i = 1, w = 0)`.
  - `w = 0`: Traceback finishes.
- **Optimal Solution Summary:**
  - **Selected Items:** **Item 2 and Item 3**.
  - **Total Weight:** `3 + 4 = 7`.
  - **Maximum Value:** `4 + 5 = 9`.

---

## Solution Q9 [10 Marks]

### Part a: LCS Table Construction (6 Marks)
`X = "ABCBDAB"`, `Y = "BDCABA"` (`m = 7, n = 6`).

**Tabulation Table L[i, j]:**

| `i \ j` | `Y:` | - | B (1) | D (2) | C (3) | A (4) | B (5) | A (6) |
|:-------:|:----:|:-:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| **-** | - | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| **A (1)** | | 0 | 0 | 0 | 0 | 1 | 1 | 1 |
| **B (2)** | | 0 | 1 | 1 | 1 | 1 | 2 | 2 |
| **C (3)** | | 0 | 1 | 1 | 2 | 2 | 2 | 2 |
| **B (4)** | | 0 | 1 | 1 | 2 | 2 | 3 | 3 |
| **D (5)** | | 0 | 1 | 2 | 2 | 2 | 3 | 3 |
| **A (6)** | | 0 | 1 | 2 | 2 | 3 | 3 | 4 |
| **B (7)** | | 0 | 1 | 2 | 2 | 3 | 4 | **4** |

### Part b: Traceback Path & Output (4 Marks)
- Cell `(7, 6)`: `X[7]='B', Y[6]='A'` (Mismatch). `L[6,6]=4`, move UP to `(6, 6)`.
- Cell `(6, 6)`: `X[6]='A', Y[6]='A'` (**Match!**). Record `'A'`, move diagonally to `(5, 5)`.
- Cell `(5, 5)`: `X[5]='D', Y[5]='B'` (Mismatch). `L[4,5]=3`, move UP to `(4, 5)`.
- Cell `(4, 5)`: `X[4]='B', Y[5]='B'` (**Match!**). Record `'B'`, move diagonally to `(3, 4)`.
- Cell `(3, 4)`: `X[3]='C', Y[4]='A'` (Mismatch). `L[3,3]=2`, move LEFT to `(3, 3)`.
- Cell `(3, 3)`: `X[3]='C', Y[3]='C'` (**Match!**). Record `'C'`, move diagonally to `(2, 2)`.
- Cell `(2, 2)`: `X[2]='B', Y[2]='D'` (Mismatch). `L[2,1]=1`, move LEFT to `(2, 1)`.
- Cell `(2, 1)`: `X[2]='B', Y[1]='B'` (**Match!**). Record `'B'`, move diagonally to `(1, 0)`.
- Output sequence in reverse order: `'B'` → `'C'` → `'B'` → `'A'`.
- **Longest Common Subsequence:** **`"BCBA"`** (Length = **4**).

---

## Solution Q10 [8 Marks]

### Part a: Held-Karp Algorithm for TSP (5 Marks)
- **Recurrence Formulation (2.5 Marks):**
  Let `C(S, j)` be the minimum cost of a simple path starting at city 1, visiting every city in subset `S ⊆ V`, and ending at city `j`:
  ```
  C({1, j}, j) = d[1, j]   (Direct path from 1 to j)

  C(S, j) = min (i ∈ S, i ≠ 1, i ≠ j) { C(S - {j}, i) + d[i, j] }
  ```
  Final tour cost:
  ```
  Optimal Tour = min (j ≠ 1) { C(V, j) + d[j, 1] }
  ```
- **Complexity Comparison (2.5 Marks):**
  - **Time Complexity:** `O(n² · 2ⁿ)` (There are `2ⁿ` subsets, `n` ending cities, and `n` predecessors to test).
  - **Space Complexity:** `O(n · 2ⁿ)` table entries.
  - *Superiority over Brute Force:* Brute-force evaluates `(n - 1)!` permutations. For `n = 20`, brute force takes `19! ≈ 1.2 × 10¹⁷` steps, whereas Held-Karp takes `20² · 2²⁰ ≈ 4.1 × 10⁸` operations, converting an impossible search into a solvable computation.

### Part b: DAG Shortest Paths in Θ(V + E) (3 Marks)
1. Perform **Topological Sort** of the vertices of the DAG in `Θ(V + E)` using DFS or Kahn’s in-degree queue algorithm.
2. Initialize `dist[source] = 0` and all other `dist[v] = ∞`.
3. Process vertices in topological order: For each vertex `u`, iterate over all outgoing edges `(u, v)` and relax:
   ```
   if dist[v] > dist[u] + weight(u, v):
       dist[v] = dist[u] + weight(u, v)
   ```
4. **Why it works in linear time:** Since the graph has no directed cycles, by the time vertex `u` is processed in topological order, all paths that could possibly reach `u` have already been fully relaxed. No vertex needs to be visited more than once, eliminating the need for priority queue operations. Time = **`Θ(V + E)`**.

---

## Solution Q11 [8 Marks]

### Part a: Optimal Binary Search Tree (OBST) (5 Marks)
- **Problem Statement (1.5 Marks):**
  Given keys `K1 < K2 < ... < Kn` with search probabilities `p1 .. pn` and dummy keys `d0 .. dn` with probabilities `q0 .. qn`, find a BST minimizing the expected number of comparisons:
  ```
  E[cost] = Sum(i=1 to n) [ (depth(Ki) + 1) · pi ] + Sum(j=0 to n) [ (depth(dj) + 1) · qj ]
  ```
- **Recurrence Relation (2.5 Marks):**
  Let `e[i, j]` be the expected search cost of optimal subtree containing keys `Ki .. Kj`:
  ```
  e[i, i - 1] = q_(i - 1)   (Base case: empty subtree, only dummy key)

  e[i, j] = min (i ≤ r ≤ j) { e[i, r - 1] + e[r + 1, j] + w(i, j) }   for i ≤ j
  ```
  Where `w(i, j)` is the sum of probabilities in subtree:
  ```
  w(i, j) = w(i, j - 1) + p_j + q_j
  ```
- **Complexities (1 Mark):**
  - Table of size `O(n²)`, filled in order of subchain length `L = 1` to `n`.
  - Trying all root candidates `r` takes `O(n)` per cell.
  - **Time Complexity:** `O(n³)`. **Space Complexity:** `O(n²)`.

### Part b: Reliability Design Problem (3 Marks)
- **Objective:** In an electrical system of `n` stages in series, the overall system functions only if every stage functions. To improve reliability, each stage `i` is provided with `m_i` identical copies in parallel.
- If individual component reliability is `r_i`, the reliability of stage `i` with `m_i` parallel components is:
  ```
  phi_i(m_i) = 1 - (1 - r_i)^(m_i)
  ```
- Overall system reliability to maximize:
  ```
  Maximize: Product(i=1 to n) [ phi_i(m_i) ]
  Subject to: Sum(i=1 to n) [ c_i · m_i ] ≤ Total Budget C
  ```
- **DP Solution:** Solved stage-by-stage backwards: `f_i(x)` represents the maximum reliability of stages `i` through `n` given remaining cost `x`. Evaluated iteratively in polynomial time.
