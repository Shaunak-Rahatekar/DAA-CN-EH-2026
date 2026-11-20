# End-Semester Examination Model Answers & Marking Scheme: DAA (CS3212)

**Course Code**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Duration**: 2 Hours | **Total Marks**: 60  
**Sources Used**: `DAA_Question_Bank_Ans_Final.docx`, `DAAOA_Question_Bank.docx`, `Syllabus_CS3212.pdf`  
**Mathematical Notation**: Universal Unicode text symbols.

---

## SECTION A SOLUTIONS (20 Marks Compulsory)

### Q1 Solution: Big-Θ Definition & Proof [4 Marks]
**Marking Scheme**:
- (a) Formal definition of Big-Θ with constants: 2 Marks
- (b) Proof with specific constants $c_1, c_2, n_0$: 2 Marks

**Answer**:
**(a) Formal Definition of Big-Θ (Theta)**:
`f(n) = Θ(g(n))` if and only if there exist positive constants `c1 > 0`, `c2 > 0`, and an integer `n0 ≥ 1` such that:
```
0 ≤ c1 · g(n) ≤ f(n) ≤ c2 · g(n)   for all n ≥ n0
```
*(Meaning: f(n) is asymptotically bounded both from above and below by g(n)).*

**(b) Proof for f(n) = 4n² + 3n = Θ(n²)**:
We must find positive constants `c1, c2, n0` such that:
```
c1 · n² ≤ 4n² + 3n ≤ c2 · n²   for all n ≥ n0
```
- **Lower Bound (`c1`)**:
  For all `n ≥ 1`, since `3n ≥ 0`:
  `4n² + 3n ≥ 4n²`.
  Thus, we can choose **`c1 = 4`** for all `n ≥ 1`.
- **Upper Bound (`c2`)**:
  For all `n ≥ 1`, `3n ≤ 3n²`.
  Therefore, `4n² + 3n ≤ 4n² + 3n² = 7n²`.
  Thus, we can choose **`c2 = 7`** for all `n ≥ 1`.
- **Conclusion**:
  For constants **`c1 = 4`**, **`c2 = 7`**, and **`n0 = 1`**:
  `4n² ≤ 4n² + 3n ≤ 7n²` holds for all `n ≥ 1`.
  Hence, by formal definition, `4n² + 3n = Θ(n²)`.

---

### Q2 Solution: Master Theorem Recurrences [4 Marks]
**Marking Scheme**:
- (a) Correct case identification and solution: 2 Marks
- (b) Extended Master theorem / non-applicability explanation: 2 Marks

**Answer**:
Master Theorem form: `T(n) = a · T(n/b) + f(n)`, with `a ≥ 1`, `b > 1`.
Critical exponent: `n^(log_b a)`.

**(a) T(n) = 4T(n/2) + n²**:
- `a = 4`, `b = 2`, `f(n) = n²`.
- Critical exponent: `n^(log_2 4) = n²`.
- Comparing `f(n)` with `n^(log_b a)`:
  Here, `f(n) = Θ(n^(log_b a)) = Θ(n²)`.
- This corresponds to **Case 2** of the Master Theorem (`f(n) = Θ(n^(log_b a) · log^k n)` with `k = 0`).
- **Solution**:
  ```
  T(n) = Θ(n^(log_b a) · log n) = Θ(n² log n)
  ```

**(b) T(n) = 2T(n/2) + n log n**:
- `a = 2`, `b = 2`, `f(n) = n log n`.
- Critical exponent: `n^(log_2 2) = n¹ = n`.
- Comparing `f(n)` with `n`: `f(n) = n log n` is asymptotically larger than `n`, but NOT polynomially larger (the ratio `(n log n) / n = log n`, which grows slower than `n^ε` for any `ε > 0`). Thus standard Case 3 does NOT apply.
- However, applying the **Extended Master Theorem (Case 2 generalized)**:
  When `f(n) = Θ(n^(log_b a) · log^k n)` with `k ≥ 0`:
  Here, `a = 2, b = 2 ⟹ n^(log_2 2) = n`, and `f(n) = n · log¹ n` (where `k = 1`).
- **Solution**:
  ```
  T(n) = Θ(n^(log_b a) · log^(k+1) n) = Θ(n · log² n)
  ```

---

### Q3 Solution: Optimal Substructure & Overlapping Subproblems [4 Marks]
**Marking Scheme**:
- Optimal Substructure definition: 1.5 Marks
- Overlapping Subproblems definition: 1.5 Marks
- Valid counter-example: 1 Mark

**Answer**:
1. **Optimal Substructure**:
   A problem exhibits optimal substructure if an optimal solution to the problem contains within it optimal solutions to its subproblems.
   *(E.g., In shortest paths, any subpath of a shortest path between u and v is itself a shortest path between its endpoints).*
2. **Overlapping Subproblems**:
   A problem has overlapping subproblems if a recursive algorithm revisits the *same* subproblems repeatedly rather than generating new subproblems at each step. Dynamic programming exploits this by solving each subproblem once and storing the result in a table.
3. **Example with Optimal Substructure but NO Overlapping Subproblems**:
   **Merge Sort** (or Binary Search):
   - Finding the sorted order of an array requires sorting its left and right subarrays (optimal substructure).
   - However, the left subarray and right subarray are completely disjoint; they never share common elements or duplicate subproblems. Thus, subproblems do NOT overlap, making Divide and Conquer appropriate rather than Dynamic Programming.

---

### Q4 Solution: Cut Definition & Why Reverse Edges are Excluded [4 Marks]
**Marking Scheme**:
- Definition of cut (S, T): 1 Mark
- Mathematical formula for cut capacity c(S, T): 1 Mark
- Clear explanation of why reverse edges are excluded: 2 Marks

**Answer**:
1. **Definition of a Cut**:
   A cut `(S, T)` in a flow network `G = (V, E)` is a partition of the vertex set `V` into two disjoint subsets `S` and `T = V - S` such that source `s ∈ S` and sink `t ∈ T`.
2. **Capacity of a Cut**:
   The capacity `c(S, T)` is the sum of capacities of edges directed strictly from `S` to `T`:
   ```
   c(S, T) = Σ Σ c(u, v)   (for all u ∈ S, v ∈ T)
   ```
3. **Why Reverse Edges (T → S) are Excluded**:
   - The capacity `c(S, T)` represents the absolute upper limit on the maximum net flow that can cross the barrier from the source partition `S` into the sink partition `T`.
   - Edges directed backwards from `T` to `S` carry flow back toward the source, which actually *reduces* the net flow crossing into `T` (`f(S, T) = flow(S → T) - flow(T → S)`).
   - Giving positive capacity credit to reverse edges would falsely inflate the barrier capacity. Therefore, reverse edges contribute strictly zero to the cut capacity `c(S, T)`.

---

### Q5 Solution: NP-Complete Definition & O(n⁴) 3-SAT Algorithm Impact [4 Marks]
**Marking Scheme**:
- Definition of NP-Complete: 2 Marks
- Consequence of O(n⁴) 3-SAT algorithm on P vs NP: 2 Marks

**Answer**:
1. **Definition of NP-Complete (NPC)**:
   A decision problem `L` is NP-Complete if:
   1. `L ∈ NP` (a solution can be verified in deterministic polynomial time).
   2. `L` is **NP-Hard** (every language `L' ∈ NP` can be reduced to `L` in polynomial time: `L' ≤_p L`).
2. **Consequence of an O(n⁴) 3-SAT Algorithm**:
   - 3-SAT is proven to be **NP-Complete**.
   - By definition of NP-Completeness, every problem `L' ∈ NP` reduces to 3-SAT in polynomial time (`L' ≤_p \text{3-SAT}`).
   - If an `O(n⁴)` deterministic algorithm is found for 3-SAT, then for any problem `L' ∈ NP`, an instance of `L'` can be transformed in polynomial time to a 3-SAT instance and solved in `O(n⁴)` time.
   - This would immediately imply that **every problem in NP is solvable in deterministic polynomial time**.
   - **Conclusion**: It would prove that **P = NP**, resolving the greatest open problem in computer science and collapsing the polynomial hierarchy!

---

## SECTION B SOLUTIONS (Answer any TWO: 16 Marks)

### Q6 Solution: D&C vs DP vs Greedy & Decision Tree Lower Bound [8 Marks]
**Marking Scheme**:
- (a) Comparison table across all 3 paradigms: 5 Marks
- (b) Adversary Decision Tree lower bound proof: 3 Marks

**Answer**:
**(a) Structured Comparison Table**:

| Feature | Divide and Conquer | Dynamic Programming | Greedy Strategy |
| :--- | :--- | :--- | :--- |
| **Subproblem Relationship** | Independent, disjoint subproblems | Overlapping subproblems with optimal substructure | Overlapping subproblems with optimal substructure |
| **Choice Strategy** | Divides problem without making greedy choices | Explores all subproblems; makes decisions bottom-up | Makes irrevocable, locally optimal choice at each step |
| **Memoization / Table** | No table needed (independent branches) | Stores solutions in a table (memoization / bottom-up) | No table needed (single path of choices) |
| **Optimality Guarantee** | Always optimal | Always optimal | Only optimal if problem has Greedy Choice Property |
| **Classic Example** | Merge Sort, Quick Sort, Karatsuba | 0/1 Knapsack, LCS, Matrix Chain | Fractional Knapsack, Prim's, Huffman |

**(b) Adversary Decision Tree Lower Bound Proof (Ω(n log n))**:
1. Any comparison-based sorting algorithm can be modeled as a binary **Decision Tree**, where each internal node represents a comparison `A[i] ≤ A[j]` and each leaf represents a sorted permutation.
2. For an array of `n` distinct elements, there are `n!` possible permutations. The decision tree must contain at least `n!` leaves to correctly sort every possible input:
   ```
   Leaves L ≥ n!
   ```
3. A binary tree of height `h` has at most `2^h` leaves:
   ```
   2^h ≥ L ≥ n!
   ```
4. Taking base-2 logarithms on both sides:
   ```
   h ≥ log_2(n!)
   ```
5. Applying **Stirling's Approximation** (`n! ≥ (n/e)^n`):
   ```
   h ≥ log_2((n/e)^n) = n · log_2 n - n · log_2 e = Ω(n log n)
   ```
6. The height `h` represents the maximum number of comparisons in the worst case. Therefore, every comparison sort requires **`Ω(n log n)`** comparisons in the worst case.

---

### Q7 Solution: 4-Queens Backtracking & Backtracking vs Branch-and-Bound [8 Marks]
**Marking Scheme**:
- (a) 4-Queens representation, bounding function, and pruning: 4 Marks
- (b) Backtracking vs Branch-and-Bound comparison: 4 Marks

**Answer**:
**(a) 4-Queens Problem Formulation & Bounding Function**:
1. **Representation**: An array `X[1..4]` where `X[i] = j` means the queen in row `i` is placed in column `j`.
2. **Constraints**:
   - No two queens in the same column: `X[i] ≠ X[k]` for all `i ≠ k`.
   - No two queens on the same diagonal: `|X[i] - X[k]| ≠ |i - k|` for all `i ≠ k`.
3. **Bounding Function `Place(k, i)`**:
   Returns `TRUE` if column `i` is safe for queen `k`, testing against all previously placed queens `j = 1 to k-1`:
   ```
   if (X[j] == i) or (abs(X[j] - i) == abs(j - k)) then return FALSE
   ```
4. **Pruning**: Whenever a queen placement violates the bounding function, the entire subtree below that node is immediately pruned (abandoned), avoiding an exhaustive search of `4⁴ = 256` states.
   - For 4-Queens, a solution is found at `X = [2, 4, 1, 3]` (Row 1 Col 2, Row 2 Col 4, Row 3 Col 1, Row 4 Col 3).

**(b) Backtracking vs Branch and Bound**:

| Parameter | Backtracking | Branch and Bound |
| :--- | :--- | :--- |
| **Tree Traversal** | Strictly **Depth-First Search (DFS)** | **Breadth-First (FIFO)** or **Best-First (LCBB / Priority Queue)** |
| **Node Expansion** | Explores one child at a time; backtracks on dead end | Expands ALL valid children of an active node at once |
| **Pruning Mechanism**| Feasibility / Bounding function (prunes invalid states) | Cost / Bounding function (prunes subtrees exceeding best known bound) |
| **Problem Types** | Decision and constraint satisfaction problems (N-Queens, Sudoku) | Optimization problems (TSP, 0/1 Knapsack, Job Assignment) |

---

### Q8 Solution: Maximum Bipartite Matching & Randomized Classes [8 Marks]
**Marking Scheme**:
- (a) Network construction, capacities, Integrality Theorem: 5 Marks
- (b) Las Vegas vs Monte Carlo comparison and examples: 3 Marks

**Answer**:
**(a) Maximum Bipartite Matching via Max Flow**:
1. **Network Construction**:
   Given bipartite graph `G = (L ∪ R, E)`:
   - Add a supersource `s` and a supersink `t`.
   - Add directed edge `s → u` for each `u ∈ L` with capacity `c(s, u) = 1`.
   - Direct each original edge `u → v` from `L` to `R` with capacity `c(u, v) = 1`.
   - Add directed edge `v → t` for each `v ∈ R` with capacity `c(v, t) = 1`.
2. **Integrality Theorem**:
   If edge capacities in a flow network are integers, then the maximum flow produced by Ford-Fulkerson assigns an integer flow to every edge.
3. **Why it Yields a Valid Matching**:
   - Since capacities are 1, flow on every edge is strictly `0` or `1`.
   - Since `c(s, u) = 1`, at most one edge leaving `u ∈ L` can carry flow 1 (each left vertex matched at most once).
   - Since `c(v, t) = 1`, at most one edge entering `v ∈ R` can carry flow 1 (each right vertex matched at most once).
   - The set of edges with `f(u, v) = 1` forms a valid matching, and `|Matching| = |f*|`.
   - Time Complexity: `O(V · E)`.

**(b) Las Vegas vs Monte Carlo Algorithms**:
1. **Las Vegas Algorithm**:
   - *Correctness*: **Always 100% correct**. Never errs.
   - *Running Time*: Random variable. Expected runtime is polynomial, but worst-case may be high.
   - *Example*: **Randomized QuickSort** (always sorts correctly; expected time `O(n log n)`).
2. **Monte Carlo Algorithm**:
   - *Correctness*: Correct with high probability, but carries a bounded probability of error.
   - *Running Time*: Deterministic / strictly bounded.
   - *Example*: **Karger's Min-Cut algorithm** (finds min-cut with probability `≥ 2/n²`; repeated runs reduce failure probability to `≤ 1/n`).

---

## SECTION C SOLUTIONS (Answer any TWO: 24 Marks)

### Q9 Solution: Dynamic Programming (LCS & 0/1 Knapsack) [12 Marks]
**Marking Scheme**:
- (a.1) Full 8×7 LCS table with values and arrows: 6 Marks
- (a.2) LCS sequence and length: 2 Marks
- (b) 0/1 Knapsack table and items selected: 4 Marks

**Answer**:
**(a) Longest Common Subsequence (LCS)**:
`X = <A, B, C, B, D, A, B>` (length m = 7)  
`Y = <B, D, C, A, B, A>` (length n = 6)

Recurrence:
- `c[i, j] = 0` if `i = 0` or `j = 0`
- `c[i, j] = c[i-1, j-1] + 1` (arrow `↖`) if `X[i] == Y[j]`
- `c[i, j] = max(c[i-1, j] (↑), c[i, j-1] (←))` if `X[i] ≠ Y[j]`

**Complete LCS Table `c[i, j]`**:

| i \ j | Y: | 0 | 1 (B) | 2 (D) | 3 (C) | 4 (A) | 5 (B) | 6 (A) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | **-** | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| **1** | **A** | 0 | 0 ↑ | 0 ↑ | 0 ↑ | 1 ↖ | 1 ← | 1 ↖ |
| **2** | **B** | 0 | 1 ↖ | 1 ← | 1 ← | 1 ↑ | 2 ↖ | 2 ← |
| **3** | **C** | 0 | 1 ↑ | 1 ↑ | 2 ↖ | 2 ← | 2 ↑ | 2 ↑ |
| **4** | **B** | 0 | 1 ↖ | 1 ↑ | 2 ↑ | 2 ↑ | 3 ↖ | 3 ← |
| **5** | **D** | 0 | 1 ↑ | 2 ↖ | 2 ↑ | 2 ↑ | 3 ↑ | 3 ↑ |
| **6** | **A** | 0 | 1 ↑ | 2 ↑ | 2 ↑ | 3 ↖ | 3 ↑ | 4 ↖ |
| **7** | **B** | 0 | 1 ↖ | 2 ↑ | 2 ↑ | 3 ↑ | 4 ↖ | 4 ↑ |

**Backtracking Trace**:
- From `(7, 6)`: value 4 (from `c[6, 6]` via `↑`)
- From `(6, 6)`: `X[6] == Y[6] == 'A'` ⟹ Take **'A'**, move to `(5, 5)` via `↖`.
- From `(5, 5)`: value 3 (from `c[4, 5]` via `↑`)
- From `(4, 5)`: `X[4] == Y[5] == 'B'` ⟹ Take **'B'**, move to `(3, 4)` via `↖`.
- From `(3, 4)`: value 2 (from `c[3, 3]` via `←`)
- From `(3, 3)`: `X[3] == Y[3] == 'C'` ⟹ Take **'C'**, move to `(2, 2)` via `↖`.
- From `(2, 2)`: value 1 (from `c[2, 1]` via `←`)
- From `(2, 1)`: `X[2] == Y[1] == 'B'` ⟹ Take **'B'**, move to `(1, 0)` via `↖`.

**Results**:
- **Length of LCS**: **4**
- **Longest Common Subsequence**: **`<B, C, B, A>`** (or `<B, D, A, B>`).

---

**(b) 0/1 Knapsack Problem**:
`W = 7`, Items:
- `Item 1: w1 = 2, v1 = 3`
- `Item 2: w2 = 3, v2 = 4`
- `Item 3: w3 = 4, v3 = 5`
- `Item 4: w4 = 5, v4 = 8`

Recurrence:
`K[i, w] = K[i-1, w]` if `w_i > w`  
`K[i, w] = max(K[i-1, w], K[i-1, w - w_i] + v_i)` if `w_i ≤ w`

**Knapsack Table `K[i, w]`**:

| i \ w | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| **1 (w=2, v=3)** | 0 | 0 | 3 | 3 | 3 | 3 | 3 | 3 |
| **2 (w=3, v=4)** | 0 | 0 | 3 | 4 | 4 | 7 | 7 | 7 |
| **3 (w=4, v=5)** | 0 | 0 | 3 | 4 | 5 | 7 | 8 | 9 |
| **4 (w=5, v=8)** | 0 | 0 | 3 | 4 | 5 | 8 | 8 | **11** |

**Backtracking for Items**:
- `K[4, 7] = 11 ≠ K[3, 7] = 9` ⟹ **Item 4 is INCLUDED**. Remaining weight: `7 - 5 = 2`.
- `K[3, 2] = 3 == K[2, 2] = 3` ⟹ Item 3 is EXCLUDED.
- `K[2, 2] = 3 == K[1, 2] = 3` ⟹ Item 2 is EXCLUDED.
- `K[1, 2] = 3 ≠ K[0, 2] = 0` ⟹ **Item 1 is INCLUDED**. Remaining weight: `2 - 2 = 0`.

**Optimal Solution**:
- **Maximum Value**: **11**
- **Items Selected**: **Item 1 and Item 4** (Total Weight = `2 + 5 = 7`, Total Value = `3 + 8 = 11`).

---

### Q10 Solution: Edmonds-Karp Network Flow & Min-Cut [12 Marks]
**Marking Scheme**:
- 1. Trace of BFS augmenting paths and residual updates: 7 Marks
- 2. Maximum flow value: 1 Mark
- 3. Reachable set S and T in final residual graph: 2 Marks
- 4. Cut capacity calculation and Max-Flow Min-Cut verification: 2 Marks

**Answer**:
Capacities: `c(s, 1)=16, c(s, 2)=13, c(1, 2)=10, c(1, 3)=12, c(2, 1)=4, c(2, 4)=14, c(3, 2)=9, c(3, t)=20, c(4, 3)=7, c(4, t)=4`.

#### 1. Edmonds-Karp Execution (BFS Shortest Path by Hops):
- **Iteration 1**:
  - BFS discovers shortest 3-edge path: `P1 = s → 1 → 3 → t` (3 edges).
  - Residual capacities: `c_f(s, 1) = 16`, `c_f(1, 3) = 12`, `c_f(3, t) = 20`.
  - Bottleneck `c_f(P1) = min(16, 12, 20) = 12`.
  - Augment flow by +12:
    `f(s, 1) = 12`, `f(1, 3) = 12` *(saturated)*, `f(3, t) = 12`.
  - Current flow `|f| = 12`.

- **Iteration 2**:
  - BFS discovers shortest 3-edge path: `P2 = s → 2 → 4 → t` (3 edges).
  - Residual capacities: `c_f(s, 2) = 13`, `c_f(2, 4) = 14`, `c_f(4, t) = 4`.
  - Bottleneck `c_f(P2) = min(13, 14, 4) = 4`.
  - Augment flow by +4:
    `f(s, 2) = 4`, `f(2, 4) = 4`, `f(4, t) = 4` *(saturated)*.
  - Current flow `|f| = 12 + 4 = 16`.

- **Iteration 3**:
  - BFS discovers 4-edge path: `P3 = s → 2 → 4 → 3 → t` (4 edges).
  - Residual capacities:
    `c_f(s, 2) = 13 - 4 = 9`
    `c_f(2, 4) = 14 - 4 = 10`
    `c_f(4, 3) = 7`
    `c_f(3, t) = 20 - 12 = 8`
  - Bottleneck `c_f(P3) = min(9, 10, 7, 8) = 7`.
  - Augment flow by +7:
    `f(s, 2) = 4 + 7 = 11`, `f(2, 4) = 4 + 7 = 11`, `f(4, 3) = 7` *(saturated)*, `f(3, t) = 12 + 7 = 19`.
  - Current flow `|f| = 16 + 7 = 23`.

- **Iteration 4 (Search in G_f)**:
  - From `s`: can reach `1` (`c_f = 4`) and `2` (`c_f = 2`).
  - From `1`: edge `(1, 3)` is saturated (0 remaining). Forward edge `(1, 2)` has capacity 10.
  - From `2`: can reach `4` (`c_f(2, 4) = 14 - 11 = 3`).
  - From `4`: `(4, t)` is saturated (0 remaining), `(4, 3)` is saturated (0 remaining).
  - No path from `s` can reach `3` or `t` in `G_f`. **Algorithm terminates.**

#### 2. Final Maximum Flow Value:
`|f*| = 12 + 4 + 7 = 23`.

#### 3. Residual Reachability & Cut Sets:
- Starting BFS from `s` in final `G_f`:
  - Reachable vertices from `s`: `{s, 1, 2, 4}`.
- **Set S**: `S = { s, 1, 2, 4 }`.
- **Set T**: `T = { 3, t }`.

#### 4. Cut Capacity Calculation & Verification:
Edges directed strictly from `S` to `T`:
1. `(1, 3)`: capacity = 12
2. `(4, 3)`: capacity = 7
3. `(4, t)`: capacity = 4
```
c(S, T) = c(1, 3) + c(4, 3) + c(4, t) = 12 + 7 + 4 = 23
```
Since `|f*| = 23 = c(S, T)`, the **Max-Flow Min-Cut Theorem** is verified!

---

### Q11 Solution: 3-SAT to Independent Set Reduction & 2-Approx VC [12 Marks]
**Marking Scheme**:
- (a) 3-SAT to Independent Set reduction construction & bidirectional proof: 6 Marks
- (b.1) 2-Approximation algorithm for Vertex Cover: 2 Marks
- (b.2) Mathematical ratio proof ρ ≤ 2: 4 Marks

**Answer**:
**(a) NP-Completeness Proof: 3-SAT ≤_p Independent Set**:
1. **Show Independent Set (IS) ∈ NP**:
   - Certificate `y`: A subset of vertices `S ⊆ V`.
   - Verifier: Checks if `|S| ≥ k` and for every pair `u, v ∈ S`, confirms `(u, v) ∉ E`. Takes `O(|V| + |E|)` polynomial time.
2. **Construction**:
   Given 3-CNF formula `φ = C1 ∧ C2 ∧ ... ∧ Cm` with `m` clauses, each with 3 literals.
   Construct graph `G = (V, E)`:
   - For each clause `C_r = (l_r1 ∨ l_r2 ∨ l_r3)`, create a triangle of 3 vertices mutually connected to each other. Total vertices `|V| = 3m`.
   - Add an edge between any two vertices representing contradictory literals `xi` and `¬xi` in different clause triangles.
   - Set target independent set size `k = m`.
   - Running time: `|V| = 3m`, `|E| ≤ 3m + 3m(3m)/2 = O(m²)`, which is polynomial.
3. **Correctness Proof**:
   - **(⇒) If φ is satisfiable**:
     Pick one literal that evaluates to TRUE from each of the `m` clauses. Select its corresponding vertex in `G`.
     This gives `m` vertices.
     Since each triangle can have at most one vertex picked (they are connected by triangle edges), we pick exactly one per clause.
     Since all chosen literals are TRUE, no two can be contradictory (`xi` and `¬xi`). Thus, no edge exists between any two chosen vertices.
     Therefore, the `m` vertices form an **Independent Set of size k = m**.
   - **(⇐) If G has an Independent Set S of size k = m**:
     Since each clause triangle has all 3 vertices mutually connected, `S` can contain at most one vertex from each of the `m` triangles.
     Because `|S| = m`, `S` must contain **exactly one vertex from each clause triangle**.
     Because `S` is an independent set, no edge exists between any vertices in `S`, meaning no two chosen vertices represent conflicting literals `xi` and `¬xi`.
     Assign value TRUE to every literal in `S`. This truth assignment satisfies at least one literal in every clause, satisfying `φ`.
   - Hence, Independent Set is **NP-Complete**.

**(b) 2-Approximation for Vertex Cover (Maximal Matching)**:
1. **Algorithm**:
   ```text
   Algorithm Approx_Vertex_Cover(G = (V, E)):
   C = ∅
   E' = E
   While E' ≠ ∅:
       Pick an arbitrary edge (u, v) ∈ E'
       Add both u and v to C:  C = C ∪ {u, v}
       Remove from E' all edges incident to u or v
   Return C
   ```
2. **Mathematical Proof of Approximation Ratio ρ ≤ 2**:
   - Let `M` be the set of edges selected by the algorithm.
   - By construction, when an edge `(u, v)` is chosen, all incident edges are removed. Therefore, no two edges in `M` share an endpoint. `M` is a **maximal matching**.
   - For each edge in `M`, both endpoints are added to `C`. Thus:
     ```
     |C| = 2 · |M|
     ```
   - Now consider the optimal (minimum) vertex cover `C*`.
   - Any valid vertex cover must cover every edge in `M`. Because the edges in `M` are completely vertex-disjoint, no single vertex in `C*` can cover more than one edge of `M`.
   - Therefore, to cover all `|M|` disjoint edges, `C*` must select at least one unique vertex for each edge in `M`:
     ```
     |C*| ≥ |M|
     ```
   - Combining the equations:
     ```
     |C| = 2 · |M| ≤ 2 · |C*|
     ```
   - Dividing by `|C*|`:
     ```
     |C| / |C*| ≤ 2
     ```
   - The algorithm runs in `O(V + E)` time and is guaranteed to produce a vertex cover at most twice the optimal size.
