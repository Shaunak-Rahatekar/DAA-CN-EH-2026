# DAA Unit I: Model Answers & Marking Scheme (Complexity Analysis)

> **Sources Used:**
> - `DAA_Question_Bank_Ans_Final.docx` (Unit I)
> - `DAA Endsem Revision (CS3212).pdf`
> - `BOOKS\Introduction to Algorithms_Cormen_3rd Ed.pdf` (CLRS)

---

## Solution Q1 [4 Marks]

### 1. Definition of Algorithm (1 Mark)
An **algorithm** is a finite sequence of well-defined, unambiguous instructions designed to solve a specific computational problem. It accepts a set of inputs, performs a finite number of deterministic steps, and produces an output.

### 2. Four Essential Criteria (Donald Knuth) (3 Marks — 0.75 Mark each)
1. **Definiteness (Unambiguity):** Each instruction must be clear, precise, and have only one possible interpretation (e.g., `x = x + 1`, not "increase x slightly").
2. **Finiteness:** For all valid inputs, the algorithm must terminate after a finite number of steps (no infinite loops).
3. **Input:** It must accept zero or more externally supplied values from a specified domain.
4. **Output:** It must produce at least one output quantity related to the input.
*(5th property: **Effectiveness:** Every operation must be basic enough to be carried out manually with pencil and paper in finite time).*

---

## Solution Q2 [4 Marks]

### Part 1: Algorithm vs Pseudocode (2 Marks)
| Criterion | Algorithm | Pseudocode |
|-----------|-----------|------------|
| **Nature** | Abstract computational procedure. | Structured textual representation of an algorithm. |
| **Language** | Mathematical notation or plain language. | Programming-like constructs (`if`, `while`) without strict syntax rules. |
| **Target** | Concept design and complexity proof. | Bridge between design and programming implementation. |

### Part 2: Time vs Space Complexity (2 Marks)
- **Time Complexity:** The number of elementary operations (comparisons, arithmetic steps) executed by an algorithm as a function of the input size `n`.
- **Space Complexity:** The total amount of memory required by an algorithm during execution as a function of input size `n`, consisting of fixed code space plus variable dynamic/recursion stack space:
  ```
  Space(n) = Fixed Space + Variable Space(n)
  ```

---

## Solution Q3 [4 Marks]

### 1. Formal Mathematical Definitions (3 Marks — 1 Mark each)
1. **Big-O Notation (O):**
   ```
   f(n) = O(g(n)) if and only if there exist constants c > 0, n0 ≥ 1 such that:
       0 ≤ f(n) ≤ c · g(g(n))   for all n ≥ n0
   ```
   *Represents an asymptotic upper bound (worst-case ceiling).*

2. **Big-Omega Notation (Ω):**
   ```
   f(n) = Ω(g(n)) if and only if there exist constants c > 0, n0 ≥ 1 such that:
       0 ≤ c · g(n) ≤ f(n)   for all n ≥ n0
   ```
   *Represents an asymptotic lower bound (best-case floor).*

3. **Big-Theta Notation (Θ):**
   ```
   f(n) = Θ(g(n)) if and only if there exist constants c1 > 0, c2 > 0, n0 ≥ 1 such that:
       0 ≤ c1 · g(n) ≤ f(n) ≤ c2 · g(n)   for all n ≥ n0
   ```
   *Represents an asymptotically tight bound (f(n) is in BOTH O(g(n)) and Ω(g(n))).*

### 2. Meaning of Constants c, n0 (1 Mark)
- `n0` is the **threshold input size** beyond which the asymptotic behavior holds true (ignoring small input anomalies).
- `c, c1, c2` are **scaling constants** that absorb machine-specific hardware speed differences.

---

## Solution Q4 [4 Marks]

### Step 1: Upper Bound Proof (O(n²)) (1.5 Marks)
We need to find `c2 > 0` such that `4n² + 3n ≤ c2 · n²` for `n ≥ n0`.
For all `n ≥ 1`:
```
3n ≤ 3n²
```
Adding `4n²` to both sides:
```
4n² + 3n ≤ 4n² + 3n² = 7n²
```
Thus, `c2 = 7` holds for all `n ≥ n0 = 1`.

### Step 2: Lower Bound Proof (Ω(n²)) (1.5 Marks)
We need to find `c1 > 0` such that `c1 · n² ≤ 4n² + 3n` for `n ≥ n0`.
Since `n ≥ 1`, `3n > 0`:
```
4n² + 3n ≥ 4n²
```
Thus, `c1 = 4` holds for all `n ≥ n0 = 1`.

### Step 3: Conclusion (1 Mark)
Combining both inequalities:
```
4n² ≤ 4n² + 3n ≤ 7n²   for all n ≥ 1
```
With `c1 = 4`, `c2 = 7`, and `n0 = 1`, by formal definition:
```
4n² + 3n = Θ(n²)   (Proved)
```

---

## Solution Q5 [4 Marks]

### 1. Order of an Algorithm (2 Marks)
The **order of an algorithm** represents the rate of growth of its execution time or memory requirement as the input size `n` tends to infinity. It characterizes how computational resource requirements scale when the problem size doubles or increases by orders of magnitude.

### 2. Why Empirical Profiling is Insufficient (2 Marks)
1. **Hardware Bias:** A program timed on a supercomputer vs a laptop produces drastically different wall-clock measurements for identical algorithm logic.
2. **Compiler & OS Interference:** Background OS processes, CPU frequency scaling, cache misses, and compiler optimization flags distort raw time.
3. **Input Sensitivity:** Profiling tests only a few specific sample inputs, whereas asymptotic analysis mathematically models all possible inputs of size `n`.

---

## Solution Q6 [4 Marks]

### 1. The Decision Tree Model (2 Marks)
- Any comparison-based sorting algorithm determines the sorted permutation of `n` elements using pairwise comparisons (`A[i] ≤ A[j]`).
- This process is modeled as a **binary decision tree**:
  - Each internal node = a comparison operation.
  - Each leaf node = one unique sorted permutation.
- For `n` distinct elements, there are `n!` possible permutations, so the tree must contain at least `n!` leaves:
  ```
  Number of Leaves L ≥ n!
  ```

### 2. Lower Bound Derivation (2 Marks)
- A binary tree of height `h` has at most `2ʰ` leaves:
  ```
  2ʰ ≥ Leaves L ≥ n!  ==>  h ≥ log₂(n!)
  ```
- Using summation expansion:
  ```
  log₂(n!) = log₂(1) + log₂(2) + ... + log₂(n)
           ≥ Sum(i = n/2 to n) [ log₂(n/2) ]
           ≥ (n / 2) · log₂(n / 2)
           = (n / 2) · [ log₂ n - 1 ]
           = Ω(n log n)
  ```
- Since height `h` represents the maximum number of comparisons in the worst case, **every comparison-based sort must make Ω(n log n) comparisons.**

---

## Solution Q7 [4 Marks]

### 1. Definition (1 Mark)
A **loop invariant** is a formal mathematical predicate regarding program variables that remains true immediately before and immediately after every iteration of a loop.

### 2. Three Mandatory Proof Stages (3 Marks — 1 Mark each)
1. **Initialization:** Show that the invariant holds true **prior to the first iteration** of the loop (base case).
2. **Maintenance:** Show that if the invariant is true before iteration `k`, it **remains true before iteration k + 1** (inductive step).
3. **Termination:** When the loop terminates, the invariant provides a guarantee that the algorithm has fulfilled its post-condition and produced the correct output.

---

## Solution Q8 [8 Marks]

### Part a: Binary Search Analysis (4 Marks)
- **Complexities (1.5 Marks):**
  - Best Case: `O(1)` (target matches the middle element on the very first comparison).
  - Worst Case: `O(log n)` (target is at the extreme end or absent).
  - Average Case: `O(log n)`.
- **Recurrence & Derivation (2.5 Marks):**
  ```
  T(n) = T(n/2) + c,   with T(1) = 1
  ```
  Using repeated substitution:
  ```
  T(n) = T(n/2) + c
       = [T(n/4) + c] + c = T(n/4) + 2c
       = [T(n/8) + c] + 2c = T(n/8) + 3c
       ...
       = T(n / 2ᵏ) + k · c
  ```
  Set `n / 2ᵏ = 1`  ==>  `2ᵏ = n`  ==>  `k = log₂ n`:
  ```
  T(n) = T(1) + c · log₂ n = 1 + c · log₂ n = Θ(log n)
  ```

### Part b: Quick Sort Worst-Case Analysis (4 Marks)
- **Cause of Worst Case (1.5 Marks):**
  Occurs when the pivot selection consistently produces the most unbalanced partitions possible: one subproblem of size `n - 1` and one of size `0`.
  *Trigger:* An array already sorted in ascending order when the first or last element is chosen as the pivot.
- **Recurrence & Solution (2.5 Marks):**
  ```
  T(n) = T(n - 1) + T(0) + c · n = T(n - 1) + c · n
  ```
  Expanding:
  ```
  T(n) = T(n - 1) + c · n
       = T(n - 2) + c · (n - 1) + c · n
       = c · [ 1 + 2 + 3 + ... + n ]
       = c · [ n(n + 1) / 2 ]
       = Θ(n²)
  ```
  *Space complexity in worst case:* `O(n)` recursion stack frames.

---

## Solution Q9 [10 Marks]

### Part a: Merge Sort Algorithm & Analysis (5 Marks)
- **Algorithm (2 Marks):**
  ```text
  MergeSort(A, p, r):
    if p < r:
      q = floor((p + r) / 2)
      MergeSort(A, p, q)
      MergeSort(A, q + 1, r)
      Merge(A, p, q, r)
  ```
  `Merge` scans both sorted halves in linear time `Θ(n)` using two pointers and merges them into an auxiliary array.
- **Recurrence & Solution (3 Marks):**
  ```
  T(n) = 2T(n/2) + c · n,   with T(1) = 1
  ```
  Using Master Theorem: `a = 2, b = 2, f(n) = c · n`.
  Critical exponent: `n^(log_b a) = n^(log₂ 2) = n¹ = n`.
  Since `f(n) = Θ(n¹)`, **Case 2 of Master Theorem applies**:
  ```
  T(n) = Θ(n · log n)   in ALL cases (Best, Average, and Worst!)
  ```
  *Auxiliary space:* `O(n)` memory needed for merging.

### Part b: Trace on Input [78, 32, 42, 62, 98, 12, 34, 83, 10] (5 Marks)
- **Splitting Stages (2.5 Marks):**
  - Initial (size 9): `[78, 32, 42, 62, 98 | 12, 34, 83, 10]`
  - Left (5): `[78, 32, 42 | 62, 98]`
    - `[78, 32 | 42]` → `[78]` and `[32]`
    - `[62 | 98]` → `[62]` and `[98]`
  - Right (4): `[12, 34 | 83, 10]`
    - `[12 | 34]` → `[12]` and `[34]`
    - `[83 | 10]` → `[83]` and `[10]`
- **Merging Stages (2.5 Marks):**
  1. Merge `[78]` & `[32]` → `[32, 78]`
  2. Merge `[32, 78]` & `[42]` → `[32, 42, 78]`
  3. Merge `[62]` & `[98]` → `[62, 98]`
  4. Merge `[32, 42, 78]` & `[62, 98]` → **`[32, 42, 62, 78, 98]`** (Left sorted)
  5. Merge `[12]` & `[34]` → `[12, 34]`
  6. Merge `[83]` & `[10]` → `[10, 83]`
  7. Merge `[12, 34]` & `[10, 83]` → **`[10, 12, 34, 83]`** (Right sorted)
  8. Final 2-way Merge:
     - Compare 32 and 10 → output `10`
     - Compare 32 and 12 → output `12`
     - Compare 32 and 34 → output `32`
     - Compare 42 and 34 → output `34`
     - Compare 42 and 83 → output `42`
     - Compare 62 and 83 → output `62`
     - Compare 78 and 83 → output `78`
     - Compare 98 and 83 → output `83`
     - Flush remaining: output `98`
- **Final Result:** `[10, 12, 32, 34, 42, 62, 78, 83, 98]`

---

## Solution Q10 [8 Marks]

### Part a: Strassen's Matrix Multiplication (5 Marks)
- **Concept (2 Marks):** Standard matrix multiplication divides two `n × n` matrices into eight `(n/2) × (n/2)` submatrices, requiring 8 recursive multiplications (`T(n) = 8T(n/2) + O(n²) = O(n³)`). Strassen discovered that by computing only **7 matrix products** (`P1` to `P7`) with 10 addition/subtraction combinations, the number of recursive multiplications drops from 8 to 7.
- **Recurrence & Complexity (3 Marks):**
  ```
  T(n) = 7T(n/2) + Θ(n²)
  ```
  Using Master Theorem: `a = 7, b = 2, f(n) = Θ(n²)`.
  ```
  n^(log_b a) = n^(log₂ 7) ≈ n^2.807
  ```
  Since `f(n) = n² = O(n^(2.807 - ε))` where `ε ≈ 0.807`, **Case 1 of Master Theorem applies**:
  ```
  T(n) = Θ(n^(log₂ 7)) ≈ Θ(n^2.81)
  ```
  This represents a significant asymptotic speedup over `O(n³)` for large matrices.

### Part b: Solve T(n) = T(n - 1) + n (3 Marks)
- **Repeated Substitution Method:**
  ```
  T(n) = T(n - 1) + n
       = [T(n - 2) + (n - 1)] + n
       = [T(n - 3) + (n - 2)] + (n - 1) + n
       ...
       = T(n - k) + Sum(i=0 to k-1) [n - i]
  ```
- Let `n - k = 1`  ==>  `k = n - 1`:
  ```
  T(n) = T(1) + [2 + 3 + 4 + ... + n]
       = 1 + [ (n(n + 1)/2) - 1 ]
       = n(n + 1) / 2
       = (n² + n) / 2
  ```
- **Conclusion:** `T(n) = Θ(n²)`.

---

## Solution Q11 [10 Marks]

### Part a: BFS vs DFS Comparison (5 Marks)
| Feature | Breadth-First Search (BFS) | Depth-First Search (DFS) |
|---------|----------------------------|--------------------------|
| **Data Structure** | Queue (FIFO). | Stack (LIFO) or recursion call stack. |
| **Traversal Strategy** | Explores neighbor-by-neighbor level-wise before moving deeper. | Explores as deep as possible along each branch before backtracking. |
| **Edge Types Discovered** | Tree edges, Cross edges. | Tree edges, Back edges (detect cycles!), Forward edges, Cross edges. |
| **Time Complexity** | `O(V + E)` (Adjacency List) / `O(V²)` (Matrix). | `O(V + E)` (Adjacency List) / `O(V²)` (Matrix). |
| **Space Complexity** | `O(V)` (queue holds entire frontier level). | `O(V)` (stack depth bounded by graph diameter/height). |
| **Primary Use Cases** | Shortest path in unweighted graphs, connected components. | Cycle detection, topological sort, strongly connected components. |

### Part b: Topological Sorting & Kahn's Algorithm (5 Marks)
- **Definition & Precondition (2 Marks):**
  Topological Sort is a linear ordering of vertices in a directed graph such that for every directed edge `(u, v)`, vertex `u` appears before `v` in the ordering.
  *Necessary and sufficient condition:* The graph must be a **Directed Acyclic Graph (DAG)**. If a cycle exists, no topological sort is possible because dependencies form a circular loop.
- **Kahn’s Algorithm (In-Degree Method) (2 Marks):**
  1. Compute the in-degree of every vertex.
  2. Initialize an empty queue `Q` and enqueue all vertices with `in-degree = 0`.
  3. While `Q` is not empty:
     a. Dequeue vertex `u`, append `u` to the topological ordering list.
     b. For each outgoing edge `(u, v)`, decrement `v`'s in-degree by 1.
     c. If `v`'s in-degree becomes 0, enqueue `v`.
  4. If the count of processed vertices `< |V|`, the graph has at least one cycle.
- **Running Time (1 Mark):**
  `Θ(V + E)` because each vertex is enqueued/dequeued once, and each edge is inspected once when in-degrees are decremented.
