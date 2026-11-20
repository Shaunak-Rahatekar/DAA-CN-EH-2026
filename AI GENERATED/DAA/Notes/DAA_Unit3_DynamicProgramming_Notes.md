# DAA Unit III: Dynamic Programming

> **Sources Used:**
> - `D:\2026 EXAM\DAA\DAA SYLLABUS.pdf` (CS3212, Unit III)
> - `D:\2026 EXAM\DAA\DAA Endsem Revision (CS3212).pdf` (Part A, Unit III)
> - `D:\2026 EXAM\DAA\DAAOA_Question_Bank.docx` (Unit III)
> - `D:\2026 EXAM\DAA\DAA_Question_Bank_Ans_Final.docx` (Unit III Model Answers)
> - `D:\2026 EXAM\DAA\BOOKS\Introduction to Algorithms_Cormen_3rd Ed.pdf` (CLRS Chapter 15, 25)

---

## 1. Fundamentals of Dynamic Programming

### 1.1 What is Dynamic Programming (DP)?
**Dynamic Programming** is an algorithm design technique used to solve optimization problems by breaking them down into simpler, overlapping subproblems, solving each subproblem once, and storing its solution in a table (memoization or tabulation) to avoid redundant recalculation.

> **Everyday Analogy:** If I ask you: "What is 1 + 1 + 1 + 1 + 1?" You count and say "5".  
> Then I write another "+ 1" on the board and ask: "What is it now?" You immediately answer "6".  
> You didn't re-count from 1 to 5 again because you **remembered** the previous subproblem answer. That is the essence of Dynamic Programming!

### 1.2 Two Essential Preconditions for DP
A problem can be solved using Dynamic Programming if and only if it exhibits two key properties:
1. **Optimal Substructure:** An optimal solution to the overall problem contains within it optimal solutions to its subproblems.
   *(Example: The shortest path from Pune to Delhi via Mumbai is made of the shortest path from Pune to Mumbai, plus the shortest path from Mumbai to Delhi).*
2. **Overlapping Subproblems:** A recursive solution visits the same identical smaller subproblems repeatedly, rather than generating new subproblems at every branch.
   *(Example: In naive Fibonacci recursion, `fib(3)` is computed dozens of times).*

### 1.3 Top-Down (Memoization) vs Bottom-Up (Tabulation)

| Feature | Top-Down (Memoization) | Bottom-Up (Tabulation) |
|---------|------------------------|------------------------|
| **Approach** | Starts from original problem; recurses downwards and caches results in a hash table or array. | Starts from base cases; fills table systematically from smallest subproblems to largest. |
| **Recursion Overhead** | Incurs function call stack overhead; risk of stack overflow for deep recursion. | Pure iteration (loops); zero recursion call stack overhead. |
| **Subproblem Evaluation** | Solves only subproblems that are actually needed by the call tree. | Solves all subproblems in the table in topological dependency order. |
| **Ease of Formulation**| Very natural to write directly from a mathematical recursive formula. | Requires figuring out the exact dependency ordering between cells. |

### 1.4 Comparison: Divide-and-Conquer vs Dynamic Programming vs Greedy

| Dimension | Divide and Conquer | Dynamic Programming | Greedy Strategy |
|-----------|--------------------|---------------------|-----------------|
| **Subproblem Overlap** | Subproblems are completely disjoint and independent (e.g., Merge Sort halves). | Subproblems heavily overlap and share sub-subproblems. | Greedy choice made upfront; no overlapping exploration. |
| **Decision Point** | Solves all subproblems before combining. | Compares solutions of multiple subproblems to pick the optimum. | Makes a single locally optimal choice at each step; never reconsiders. |
| **Guaranteed Optimality** | Yes, if base cases and merges are correct. | Always finds global optimum via exhaustive optimal substructure. | Finds global optimum ONLY if greedy-choice property holds. |
| **Backtracking/Reconsideration** | No backtracking. | Stores all past decisions in table. | No backtracking once choice is committed. |

---

## 2. Classic Dynamic Programming Problems

---

### 2.1 Matrix Chain Multiplication (MCM)
> **Source:** `DAAOA_Question_Bank.docx`, Unit III, Q17 & `DAA Endsem Revision`, Part C, Q13

- **Problem:** Given a sequence of matrices `A1, A2, ..., An` where matrix `Ai` has dimensions `p_(i-1) × p_i`, find the parenthesization that minimizes the total scalar multiplications.
- **Why Parenthesization Matters:**
  Matrix multiplication is associative (`(A·B)·C = A·(B·C)`), but cost varies drastically:
  If `A` is `10 × 100`, `B` is `100 × 5`, and `C` is `5 × 50`:
  - `(A·B)·C` takes `(10 × 100 × 5) + (10 × 5 × 50) = 5,000 + 2,500 = 7,500` multiplications.
  - `A·(B·C)` takes `(100 × 5 × 50) + (10 × 100 × 50) = 25,000 + 50,000 = 75,000` multiplications! (10× cost difference!).
- **Number of Parenthesizations:** Catalan number `C(n-1) = (2(n-1))! / ((n-1)! · n!) = Ω(4ⁿ / n^1.5)`. Brute force is exponential.

#### The MCM Recurrence:
Let `m[i, j]` be the minimum scalar multiplications needed to compute `Ai ... Aj`:
```
m[i, j] = 0   if i == j (single matrix)

m[i, j] = min (i ≤ k < j) { m[i, k] + m[k+1, j] + p_(i-1) · p_k · p_j }   for i < j
```
Where `k` is the split point that divides the chain into `(Ai .. Ak)` and `(A_(k+1) .. Aj)`.
Store the optimal split `k` in table `s[i, j]`.

#### Complexity:
- Subproblems: `n(n+1)/2 = O(n²)`.
- Work per subproblem: loop over `k` from `i` to `j - 1` takes `O(n)`.
- **Total Time Complexity:** `O(n³)`.
- **Total Space Complexity:** `O(n²)` for `m` and `s` tables.

#### Worked Exam Example:
> **Question Bank Instance:** Chain of 4 matrices with dimensions:
> `A1: 3 × 2`, `A2: 2 × 4`, `A3: 4 × 2`, `A4: 2 × 5`  
> Dimensions array `p = [3, 2, 4, 2, 5]` (`n = 4`).

1. **Chain length L = 1:** `m[1,1] = m[2,2] = m[3,3] = m[4,4] = 0`.
2. **Chain length L = 2:**
   - `m[1, 2] = m[1,1] + m[2,2] + p0·p1·p2 = 0 + 0 + (3 · 2 · 4) = 24` (split `s[1,2] = 1`).
   - `m[2, 3] = m[2,2] + m[3,3] + p1·p2·p3 = 0 + 0 + (2 · 4 · 2) = 16` (split `s[2,3] = 2`).
   - `m[3, 4] = m[3,3] + m[4,4] + p2·p3·p4 = 0 + 0 + (4 · 2 · 5) = 40` (split `s[3,4] = 3`).
3. **Chain length L = 3:**
   - `m[1, 3] = min(`
     - `k=1: m[1,1] + m[2,3] + p0·p1·p3 = 0 + 16 + (3 · 2 · 2) = 28`,
     - `k=2: m[1,2] + m[3,3] + p0·p2·p3 = 24 + 0 + (3 · 4 · 2) = 48`
     `) = 28` (split `s[1,3] = 1`).
   - `m[2, 4] = min(`
     - `k=2: m[2,2] + m[3,4] + p1·p2·p4 = 0 + 40 + (2 · 4 · 5) = 80`,
     - `k=3: m[2,3] + m[4,4] + p1·p3·p4 = 16 + 0 + (2 · 2 · 5) = 36`
     `) = 36` (split `s[2,4] = 3`).
4. **Chain length L = 4 (Full Chain m[1, 4]):**
   - `k=1: m[1,1] + m[2,4] + p0·p1·p4 = 0 + 36 + (3 · 2 · 5) = 66`
   - `k=2: m[1,2] + m[3,4] + p0·p2·p4 = 24 + 40 + (3 · 4 · 5) = 124`
   - `k=3: m[1,3] + m[4,4] + p0·p3·p4 = 28 + 0 + (3 · 2 · 5) = 58`
   - Minimum is **`58`** at split `k = 3` (`s[1,4] = 3`).
- **Optimal Parenthesization:** `s[1,4] = 3` splits into `(A1 A2 A3) (A4)`. Then `s[1,3] = 1` splits into `(A1) (A2 A3)`.
  **Result:** `((A1 · (A2 · A3)) · A4)` with **58** scalar multiplications!

---

### 2.2 The 0/1 Knapsack Problem
> **Source:** `DAAOA_Question_Bank.docx`, Unit III, Q18 & `DAA_Question_Bank_Ans_Final.docx`

- **Problem:** Given `n` items, each with weight `w_i` and value `v_i`, and a knapsack of maximum weight capacity `W`. Maximize total value without exceeding capacity `W`. Each item can either be taken completely (`1`) or left behind (`0`). No fractions allowed!

#### The Recurrence Relation:
Let `K[i, w]` be the maximum value achievable using a subset of the first `i` items with total weight `≤ w`:
```
K[i, w] = K[i - 1, w]   if w_i > w (item i is too heavy)

K[i, w] = max( K[i - 1, w],   v_i + K[i - 1, w - w_i] )   if w_i ≤ w
```
- Base Cases: `K[0, w] = 0` for all `w`, and `K[i, 0] = 0` for all `i`.

#### Complexity:
- Table dimensions: `(n + 1) × (W + 1)`.
- Time to fill each cell: `O(1)`.
- **Time Complexity:** `O(n · W)`. (This is **pseudo-polynomial** because it depends on the numerical value of `W`, not just the number of input bits).
- **Space Complexity:** `O(n · W)` table (can be optimized to `O(W)` space using 1D array).

#### Worked Exam Example (Capacity W = 7):
Items:
- Item 1: `w1 = 1, v1 = 1`
- Item 2: `w2 = 3, v2 = 4`
- Item 3: `w3 = 4, v3 = 5`
- Item 4: `w4 = 5, v4 = 7`
Capacity `W = 7`.

**Tabulation Table K[i, w]:**

| Item `i` | (w, v) | w=0 | w=1 | w=2 | w=3 | w=4 | w=5 | w=6 | w=7 |
|:--------:|:------:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **0** | None | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| **1** | (1, 1) | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| **2** | (3, 4) | 0 | 1 | 1 | 4 | 5 | 5 | 5 | 5 |
| **3** | (4, 5) | 0 | 1 | 1 | 4 | 5 | 6 | 6 | 9 |
| **4** | (5, 7) | 0 | 1 | 1 | 4 | 5 | 7 | 8 | **9** |

- **Maximum Profit:** `K[4, 7] = 9`.
- **Item Selection Traceback:**
  - `K[4, 7] == K[3, 7] == 9`: Item 4 was **NOT** taken. Move to `K[3, 7]`.
  - `K[3, 7] (9) ≠ K[2, 7] (5)`: Item 3 **WAS taken**! Value += 5, remaining weight = `7 - 4 = 3`. Move to `K[2, 3]`.
  - `K[2, 3] (4) ≠ K[1, 3] (1)`: Item 2 **WAS taken**! Value += 4, remaining weight = `3 - 3 = 0`.
  - **Selected Items:** **Item 2 and Item 3** (Total weight = `3 + 4 = 7`, Total value = `4 + 5 = 9`).

---

### 2.3 Longest Common Subsequence (LCS)
> **Source:** `DAA Endsem Revision (CS3212).pdf`, Part C, Q12

- **Problem:** Given two sequences `X = [x1 .. xm]` and `Y = [y1 .. yn]`, find the length and characters of the longest subsequence present in both (characters need not be contiguous, but must maintain relative order).

#### The Recurrence Relation:
Let `L[i, j]` be the length of the LCS of prefixes `X[1 .. i]` and `Y[1 .. j]`:
```
L[i, j] = 0   if i == 0 or j == 0

L[i, j] = L[i - 1, j - 1] + 1   if X[i] == Y[j] (match! extend LCS)

L[i, j] = max( L[i - 1, j],  L[i, j - 1] )   if X[i] != Y[j] (no match! take best of discarding one char)
```

#### Worked Exam Example:
`X = "ABCBDAB"` (`m = 7`), `Y = "BDCABA"` (`n = 6`)

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

- **Length of LCS:** `L[7, 6] = 4`.
- **Traceback (Starting at bottom-right cell L[7, 6]):**
  - `X[7] = 'B'`, `Y[6] = 'A'` (Mismatch): `L[7,6] = 4`, neighbors `L[6,6]=4, L[7,5]=4`. Move up to `(6, 6)`.
  - `X[6] = 'A'`, `Y[6] = 'A'` (**Match!**): Take `'A'`, move diagonally up-left to `(5, 5)`.
  - `X[5] = 'D'`, `Y[5] = 'B'` (Mismatch): `L[5,5]=3, L[4,5]=3, L[5,4]=2`. Move up to `(4, 5)`.
  - `X[4] = 'B'`, `Y[5] = 'B'` (**Match!**): Take `'B'`, move diagonally to `(3, 4)`.
  - `X[3] = 'C'`, `Y[4] = 'A'` (Mismatch): `L[3,4]=2, L[2,4]=1, L[3,3]=2`. Move left to `(3, 3)`.
  - `X[3] = 'C'`, `Y[3] = 'C'` (**Match!**): Take `'C'`, move diagonally to `(2, 2)`.
  - `X[2] = 'B'`, `Y[2] = 'D'` (Mismatch): Move left to `(2, 1)`.
  - `X[2] = 'B'`, `Y[1] = 'B'` (**Match!**): Take `'B'`, move diagonally to `(1, 0)`.
- **Recovered Longest Common Subsequence:** **`"BCBA"`** (or alternatively `"BDAB"`). Length = **4**.
- **Complexities:** Time = `O(m · n)`, Space = `O(m · n)`.

---

### 2.4 All-Pairs Shortest Path: Floyd-Warshall Algorithm
> **Source:** `DAAOA_Question_Bank.docx`, Unit III, Q15(a) & `DAA Endsem Revision`, Part C, Q17

- **Problem:** Find the shortest path between every pair of vertices `(i, j)` in a directed graph `G = (V, E)`. Handles negative edge weights, but graph must contain **no negative weight cycles**.
- **Core DP Idea:** In phase `k`, consider whether path from `i` to `j` can be shortened by using vertex `k` as an intermediate waypoint.

#### The Floyd-Warshall Recurrence:
Let `D^(k)[i, j]` be the length of the shortest path from `i` to `j` using only intermediate vertices from the set `{1, 2, ..., k}`:
```
D^(k)[i, j] = min( D^(k-1)[i, j],   D^(k-1)[i, k] + D^(k-1)[k, j] )
```
- **Base Case (k = 0):** `D^(0)` is the graph's direct adjacency weight matrix `W`:
  - `D^(0)[i, i] = 0`
  - `D^(0)[i, j] = weight(i, j)` if edge exists, else `∞`.
- **Negative Cycle Detection:** If after phase `k = n`, any diagonal entry `D[i, i] < 0`, a **negative weight cycle exists**!
- **Complexities:**
  - 3 nested loops: `for k = 1 to n`, `for i = 1 to n`, `for j = 1 to n`.
  - **Time Complexity:** `Θ(V³)`.
  - **Space Complexity:** `O(V²)` matrix.

---

### 2.5 Optimal Binary Search Tree (OBST)
> **Source:** `DAAOA_Question_Bank.docx`, Unit III, Q14(a)

- **Problem:** Given sorted keys `K1 < K2 < ... < Kn` with search probabilities `p1, p2, ..., pn`, and dummy search failure keys `d0, d1, ..., dn` with probabilities `q0, q1, ..., qn`, organize keys in a Binary Search Tree that minimizes the expected total search cost.
- **Expected Search Cost Formula:**
  ```
  Expected Cost = Sum(i=1 to n) [ depth(Ki) · pi ] + Sum(j=0 to n) [ depth(dj) · qj ]
  ```
- **DP Recurrence:**
  Let `e[i, j]` be the expected search cost of an optimal BST containing keys `Ki .. Kj`:
  ```
  e[i, j] = q_(i-1)   if j = i - 1 (empty subtree)

  e[i, j] = min (i ≤ r ≤ j) { e[i, r - 1] + e[r + 1, j] + w(i, j) }   for i ≤ j
  ```
  Where `w(i, j)` is the sum of probabilities of all keys and dummies in the range:
  ```
  w(i, j) = w(i, j - 1) + p_j + q_j
  ```
- **Complexities:** Evaluates `O(n²)` subtrees, each trying `O(n)` root candidates `r` → **Time: `O(n³)`**, **Space: `O(n²)`**.

---

### 2.6 Travelling Salesperson Problem (TSP) using DP (Held-Karp)
> **Source:** `DAAOA_Question_Bank.docx`, Unit III, Q11(a)

- **Problem:** Given `n` cities and distance matrix `d[i, j]`, find the minimum cost tour that starts at city 1, visits every city exactly once, and returns to city 1.
- **Naive Permutation Cost:** `(n - 1)! = O(n!)` (intractable for `n > 15`).
- **Held-Karp Dynamic Programming:**
  Let `C(S, j)` be the minimum cost of a path starting at city 1, visiting every city in subset `S` exactly once, and ending at city `j` (where `1 ∈ S` and `j ∈ S`):
  ```
  C({1, j}, j) = d[1, j]   (Base case: direct trip from city 1 to j)

  C(S, j) = min (i ∈ S, i ≠ 1, i ≠ j) { C(S - {j}, i) + d[i, j] }
  ```
- **Final Tour Cost:**
  ```
  Optimal Tour = min (j ≠ 1) { C(V, j) + d[j, 1] }
  ```
- **Complexities:**
  - Number of subsets `S`: `2ⁿ`.
  - Cities `j`: `n`.
  - **Time Complexity:** `O(n² · 2ⁿ)`. (Exponential, but vastly superior to `O(n!)`: for `n=20`, `20! ≈ 2.4 × 10¹⁸` while `20² · 2²⁰ ≈ 4.1 × 10⁸`).
  - **Space Complexity:** `O(n · 2ⁿ)`.

---

### 2.7 Other Syllabus DP Problems

#### 1. Longest Increasing Subsequence (LIS)
- **Recurrence:** `L[i] = 1 + max { L[j] }` for all `j < i` where `A[j] < A[i]`.
- **Time Complexity:** `O(n²)` with DP; `O(n log n)` with Patience Sorting + Binary Search.

#### 2. Coin Change Problem (Minimum Coins for Value V)
- **Recurrence:** `C[v] = 1 + min (c_i ≤ v) { C[v - c_i] }`, with `C[0] = 0`.
- **Time Complexity:** `O(V · n)`.

#### 3. Single Source Shortest Path in a DAG
- **Algorithm:**
  1. Topologically sort the vertices of the DAG in `O(V + E)`.
  2. For each vertex `u` in topological order, relax all outgoing edges `(u, v)`:
     ```
     if dist[v] > dist[u] + weight(u, v):
         dist[v] = dist[u] + weight(u, v)
     ```
- **Time Complexity:** **`Θ(V + E)`** (faster than Dijkstra's `O((V + E) log V)` because no heap is needed).

#### 4. Largest Independent Set in a Tree
- **Property:** An Independent Set contains no two adjacent vertices.
- **DP on Tree:** For vertex `v`:
  ```
  I(v) = max( 1 + Sum (I(grandchildren)),   Sum (I(children)) )
  ```
- **Time Complexity:** `O(n)` using bottom-up post-order tree traversal.
