# DAA Unit I: Basic Introduction and Time & Space Complexity Analysis

> **Sources Used:**
> - `D:\2026 EXAM\DAA\DAA SYLLABUS.pdf` (CS3212, Unit I)
> - `D:\2026 EXAM\DAA\DAA Endsem Revision (CS3212).pdf` (Part A, Unit I)
> - `D:\2026 EXAM\DAA\DAAOA_Question_Bank.docx` (Unit I, Q1 to Q19)
> - `D:\2026 EXAM\DAA\DAA_Question_Bank_Ans_Final.docx` (Unit I Model Answers)
> - `D:\2026 EXAM\DAA\BOOKS\Introduction to Algorithms_Cormen_3rd Ed.pdf` (CLRS Chapters 1, 2, 3)

---

## 1. Fundamentals of Algorithms

### 1.1 What is an Algorithm?
An **algorithm** is a finite sequence of unambiguous, step-by-step instructions designed to take an input, process it through logical operations, and produce a valid output in a finite amount of time.

> **Everyday Analogy:** Think of an algorithm like a kitchen recipe. It lists raw ingredients (input), numbered cooking steps in order (process), and delivers a prepared meal (output). If the recipe never finishes or says vague things like "bake until magical", it is not a valid algorithm.

### 1.2 The 5 Mandatory Criteria of a Good Algorithm (Donald Knuth Criteria)
Every valid algorithm must satisfy these five criteria:

| # | Criterion | Meaning | Practical Exam Example |
|---|-----------|---------|-------------------------|
| 1 | **Input** | Accepts zero or more externally supplied quantities from a specified set. | An array of numbers `A[1 .. n]`. |
| 2 | **Output** | Produces at least one quantity as a result of computation. | Sorted array or index of target element. |
| 3 | **Definiteness** | Every instruction must be clear, precise, and completely unambiguous. | `i = i + 1` (NOT "increase i slightly"). |
| 4 | **Finiteness** | The algorithm must terminate after a finite number of steps for all inputs. | A loop with condition `i < n` and increment `i = i + 1`. |
| 5 | **Effectiveness** | Each operation must be basic enough to be carried out manually with pencil and paper in finite time. | Basic addition, comparison, assignment. |

### 1.3 Algorithm vs Pseudocode vs Program

| Feature | Algorithm | Pseudocode | Program |
|---------|-----------|------------|---------|
| **Definition** | Conceptual, mathematical step-by-step method to solve a problem. | High-level, informal structured text representation of an algorithm. | Concrete code implemented in an executable programming language (C++, Java, Python). |
| **Language** | Plain English / mathematical logic. | Structured programming keywords (`if`, `while`) without strict syntax rules. | Strict syntax and keywords of the specific language. |
| **Execution** | Cannot be executed by a computer. | Cannot be executed directly. | Compiled or interpreted and executed by the CPU. |
| **Audience** | Mathematicians, algorithm designers. | Software engineers and developers. | Computer CPU / runtime system. |

---

## 2. Need for Performance Analysis: Time & Space Complexity

### 2.1 Why Analyze Algorithms?
1. **Predictability:** We need to know how execution time scales as data grows from 10 items to 100,000,000 items.
2. **Hardware Independence:** Machine clocks, memory bandwidth, and operating systems vary. Asymptotic analysis provides a pure mathematical metric independent of computer hardware.
3. **Algorithm Comparison:** When multiple algorithms solve the same problem (e.g., Bubble Sort vs Merge Sort), analysis tells us mathematically which one scales better.

### 2.2 Definitions: Time vs Space Complexity
- **Time Complexity:** The amount of computer time (quantified by the number of basic operations like comparisons, additions, array lookups) executed by an algorithm as a function of the input size `n`.
- **Space Complexity:** The total memory required by an algorithm during execution as a function of input size `n`:
  ```
  Total Space = Fixed Space (code + constants) + Variable Space (dynamic arrays + recursion stack)
  ```

### 2.3 Efficiency Cases

| Case | Definition | Practical Meaning | Example: Linear Search in Array of Size n |
|------|------------|-------------------|------------------------------------------|
| **Best Case** | Minimum operations performed over any input of size `n`. | Ideal scenario. | Target item found at index 0 → `O(1)`. |
| **Worst Case** | Maximum operations performed over any input of size `n`. | Upper bound guarantee. **Standard metric required in university exams.** | Target at last index or absent → `O(n)`. |
| **Average Case** | Expected operations averaged over all valid inputs of size `n`. | Realistic everyday behavior (requires probability distribution). | Target found at index `n/2` on average → `O(n)`. |

---

## 3. Asymptotic Notations (Formal Definitions & Meaning)

Asymptotic notation characterizes how runtime grows as input size `n` approaches infinity (`n → ∞`).

### 3.1 Big-O Notation (O) — Asymptotic Upper Bound
- **Formal Definition:**
  ```
  f(n) = O(g(n)) if and only if there exist positive constants c > 0 and n0 ≥ 1
  such that:
      0 ≤ f(n) ≤ c · g(n)   for all n ≥ n0
  ```
- **Meaning:** `f(n)` grows no faster than `g(n)` for large `n`. It defines an upper ceiling on running time.
- **Analogy:** "My travel time will be at most 3 hours."

### 3.2 Big-Omega Notation (Ω) — Asymptotic Lower Bound
- **Formal Definition:**
  ```
  f(n) = Ω(g(n)) if and only if there exist positive constants c > 0 and n0 ≥ 1
  such that:
      0 ≤ c · g(n) ≤ f(n)   for all n ≥ n0
  ```
- **Meaning:** `f(n)` grows at least as fast as `g(n)` for large `n`. It defines a lower floor on running time.
- **Analogy:** "My travel time will take at least 1 hour."

### 3.3 Big-Theta Notation (Θ) — Asymptotic Tight Bound
- **Formal Definition:**
  ```
  f(n) = Θ(g(n)) if and only if there exist positive constants c1 > 0, c2 > 0, and n0 ≥ 1
  such that:
      0 ≤ c1 · g(n) ≤ f(n) ≤ c2 · g(n)   for all n ≥ n0
  ```
- **Key Theorem:**
  ```
  f(n) = Θ(g(n)) if and only if f(n) = O(g(n)) AND f(n) = Ω(g(n))
  ```
- **Meaning:** `f(n)` is tightly bounded between `c1 · g(n)` and `c2 · g(n)`.

### 3.4 Little-o Notation (o) — Strict Upper Bound
- **Formal Definition:**
  ```
  f(n) = o(g(n)) if for every positive constant c > 0, there exists n0 ≥ 1
  such that:
      0 ≤ f(n) < c · g(n)   for all n ≥ n0
  ```
  Equivalent limit definition:
  ```
  limit as n → ∞ of [ f(n) / g(n) ] = 0
  ```
- **Difference from Big-O:** Big-O allows the curves to grow at identical rates (e.g., `2n² = O(n²)` is true), but little-o requires strict domination (e.g., `2n = o(n²)`, but `2n² ≠ o(n²)`).

### 3.5 Growth Order of Standard Functions
Memorize this ordering from smallest to largest:
```
1 < log n < √n < n < n log n < n² < n³ < 2ⁿ < n! < nⁿ
```

---

## 4. Standard University Exam Proofs (Step-by-Step)

### Proof 1: Prove that 4n² + 3n = Θ(n²)
> **Source:** `DAAOA_Question_Bank.docx`, Unit I, Q19

**Goal:** Find positive constants `c1`, `c2`, and `n0` such that:
```
c1 · n² ≤ 4n² + 3n ≤ c2 · n²   for all n ≥ n0
```

**Step 1: Find Upper Bound Constant (c2)**
For all `n ≥ 1`, we know that `3n ≤ 3n²`.
Adding `4n²` to both sides:
```
4n² + 3n ≤ 4n² + 3n² = 7n²
```
Therefore, `c2 = 7` holds for all `n ≥ n0 = 1`.

**Step 2: Find Lower Bound Constant (c1)**
For all `n ≥ 1`, since `3n > 0`:
```
4n² + 3n ≥ 4n²
```
Therefore, `c1 = 4` holds for all `n ≥ n0 = 1`.

**Step 3: Combine Inequalities**
```
4n² ≤ 4n² + 3n ≤ 7n²   for all n ≥ 1
```
Here `c1 = 4`, `c2 = 7`, and `n0 = 1`.
**Conclusion:** By the formal definition of Big-Theta, `4n² + 3n = Θ(n²)`. (Proved)

---

### Proof 2: Prove that 3n² + 5n = Θ(n²)
> **Source:** `DAA Endsem Revision (CS3212).pdf`, Part C, Q1

**Goal:** Find `c1, c2, n0` such that `c1 · n² ≤ 3n² + 5n ≤ c2 · n²` for all `n ≥ n0`.

**Step 1: Upper Bound**
For `n ≥ 1`, `5n ≤ 5n²`:
```
3n² + 5n ≤ 3n² + 5n² = 8n²   ==> c2 = 8, n0 = 1
```

**Step 2: Lower Bound**
For `n ≥ 1`, `5n ≥ 0`:
```
3n² + 5n ≥ 3n²   ==> c1 = 3, n0 = 1
```

**Step 3: Conclusion**
```
3n² ≤ 3n² + 5n ≤ 8n²   for all n ≥ 1   ==> 3n² + 5n = Θ(n²). (Proved)
```

---

## 5. Analysis of Non-Recursive & Recursive Algorithms

### 5.1 Non-Recursive Algorithm Analysis
Count the number of times the innermost basic operation is executed using summation equations.

#### Example: Matrix Multiplication (Three Nested Loops)
```c
for (int i = 0; i < n; i++)          // runs n times
    for (int j = 0; j < n; j++)      // runs n times
        for (int k = 0; k < n; k++)  // runs n times
            C[i][j] += A[i][k] * B[k][j]; // 1 multiplication, 1 addition
```
Total basic operations executed:
```
Total = Sum(i=0 to n-1) [ Sum(j=0 to n-1) [ Sum(k=0 to n-1) 1 ] ]
      = n · n · n = n³ = Θ(n³)
```

---

### 5.2 Recursive Algorithm Analysis: Recurrence Relations

#### 1. Binary Search
- **Recurrence:** In each recursive step, we perform 1 comparison and cut the remaining search array in half:
  ```
  T(n) = T(n/2) + 1,   with base case T(1) = 1
  ```
- **Solving by Repeated Substitution:**
  ```
  T(n) = T(n/2) + 1
       = [T(n/4) + 1] + 1 = T(n/4) + 2
       = [T(n/8) + 1] + 2 = T(n/8) + 3
       ...
       = T(n / 2ᵏ) + k
  ```
  Set `n / 2ᵏ = 1`  ==>  `2ᵏ = n`  ==>  `k = log₂ n`:
  ```
  T(n) = T(1) + log₂ n = 1 + log₂ n = Θ(log n)
  ```
- **Summary:**
  - Best Case: Target at midpoint on first check → `O(1)`.
  - Worst Case: Target at leaf or absent → `O(log n)`.
  - Space Complexity: `O(1)` iterative; `O(log n)` recursive call stack.

---

#### 2. Quick Sort
- **Partitioning:** Scans the array and places the pivot in its final sorted position in linear time `c · n`.
- **Worst-Case Analysis:**
  - Occurs when the pivot produces the most unbalanced partition possible (size `0` and size `n - 1`), such as an array already sorted in ascending order when the first element is chosen as pivot.
  - Recurrence:
    ```
    T(n) = T(n - 1) + T(0) + c · n = T(n - 1) + c · n
    ```
  - Unfolding:
    ```
    T(n) = c · n + c · (n - 1) + c · (n - 2) + ... + c · 1
         = c · [ 1 + 2 + 3 + ... + n ]
         = c · [ n(n + 1) / 2 ]
         = Θ(n²)
    ```
  - Worst-case space complexity: `O(n)` recursion stack frames.
- **Best-Case Analysis:**
  - Pivot splits the array into two equal halves of size `n/2` each step:
    ```
    T(n) = 2T(n/2) + c · n  ==>  Θ(n log n)
    ```
- **Average-Case Analysis:**
  - Averaged over all `n!` random permutations, expected comparisons = `Θ(n log n)`. Expected call stack space = `O(log n)`.

---

#### 3. Merge Sort
- **Recurrence:** Divides array into two halves of size `n/2`, recursively sorts them, and merges the two sorted subarrays in linear time:
  ```
  T(n) = 2T(n/2) + c · n,   with base case T(1) = 1
  ```
- **Solving by Master Theorem:**
  - `a = 2`, `b = 2`, `f(n) = c · n`.
  - Critical function: `n^(log_b a) = n^(log₂ 2) = n¹ = n`.
  - Since `f(n) = Θ(n¹)`, Case 2 of Master Theorem applies:
    ```
    T(n) = Θ(n · log n)   in ALL cases (Best, Average, and Worst!)
    ```
- **Space Complexity:** Needs an auxiliary temporary array of size `n` during merging → `O(n)` auxiliary memory.

---

#### 4. Recurrence: T(n) = T(n - 1) + n
> **Source:** `DAAOA_Question_Bank.docx`, Unit I, Q18
- Expand by repeated substitution:
  ```
  T(n) = T(n - 1) + n
       = [T(n - 2) + (n - 1)] + n
       = [T(n - 3) + (n - 2)] + (n - 1) + n
       ...
       = T(n - k) + Sum(i=0 to k-1) [n - i]
  ```
  Let `n - k = 1`  ==>  `k = n - 1`:
  ```
  T(n) = T(1) + [2 + 3 + 4 + ... + n]
       = 1 + [ (n(n + 1)/2) - 1 ]
       = n(n + 1) / 2
       = Θ(n²)
  ```

---

## 6. Overview of Basic Algorithms: Graph Traversals & Topological Sort

### 6.1 Breadth-First Search (BFS)
- **Data Structure:** Queue (FIFO — First In, First Out).
- **Strategy:** Explores neighbor-by-neighbor, visiting all vertices at distance `d` before any at distance `d + 1`.
- **Time Complexity:** `O(V + E)` with Adjacency List; `O(V²)` with Adjacency Matrix.
- **Space Complexity:** `O(V)` to store visited array and queue elements.
- **Core Applications:** Shortest path in unweighted graphs, connected components, bipartite testing.

### 6.2 Depth-First Search (DFS)
- **Data Structure:** Stack (LIFO) or recursive call stack.
- **Strategy:** Explores as deeply as possible along each branch before backtracking.
- **Time Complexity:** `O(V + E)` with Adjacency List.
- **Space Complexity:** `O(V)` for recursion stack depth.
- **Edge Types in DFS Tree:**
  1. *Tree edge:* Leads to an unvisited vertex.
  2. *Back edge:* Leads to an ancestor in the DFS tree (indicates a **cycle**!).
  3. *Forward edge:* Leads to a descendant already visited.
  4. *Cross edge:* Leads to a vertex in another subtree without an ancestor relation.
- **Core Applications:** Cycle detection in graphs, topological sorting, finding strongly connected components.

### 6.3 Topological Sorting
- **Precondition:** Defined **ONLY** for Directed Acyclic Graphs (DAG). A graph with even one directed cycle has no topological sort.
- **Definition:** A linear ordering of vertices such that for every directed edge `(u, v)`, vertex `u` appears before vertex `v` in the sequence.
- **Algorithms:**
  1. **DFS Finish-Time Method:** Run DFS. When a vertex finishes (all its outgoing edges are explored), prepend it to a linked list. The reverse of finish times produces the topological sort. Running time: `Θ(V + E)`.
  2. **Kahn's Algorithm (In-degree queue method):**
     - Step 1: Compute in-degree for all vertices.
     - Step 2: Enqueue all vertices with in-degree = 0.
     - Step 3: While queue is not empty, dequeue `u`, output `u`, and decrement in-degree of all its neighbors `v`.
     - Step 4: If a neighbor's in-degree reaches 0, enqueue it.
     - Step 5: If output list contains `< |V|` vertices, a cycle was detected.
     - Running time: `O(V + E)`.

---

## 7. Adversary Lower Bound Technique (Comparison Sorting Lower Bound)

> **Syllabus Topic:** Adversary lower bound technique for comparison-based sorting (`Ω(n log n)`).

### 7.1 The Binary Decision Tree Model
Any algorithm that sorts by comparing elements (`A[i] ≤ A[j]`) can be modeled as a binary decision tree:
- Each **internal node** represents a comparison between two elements `A[i] : A[j]`.
- Each branch represents the outcome (`≤` or `>`).
- Each **leaf node** represents one possible sorted permutation of the `n` input elements.
- For `n` distinct items, there are `n!` possible permutations.
- To produce the correct answer for every input, the tree must contain at least `n!` leaves:
  ```
  Number of Leaves L ≥ n!
  ```

### 7.2 Mathematical Derivation of Ω(n log n)
Let `h` be the height of the decision tree (which equals the maximum number of comparisons in the worst case):
- A binary tree of height `h` has at most `2ʰ` leaves:
  ```
  2ʰ ≥ Leaves L ≥ n!
  ```
- Taking base-2 logarithm on both sides:
  ```
  h ≥ log₂(n!)
  ```
- Using summation expansion:
  ```
  log₂(n!) = log₂(1) + log₂(2) + ... + log₂(n)
           ≥ Sum(i = n/2 to n) [ log₂(n/2) ]
           ≥ (n / 2) · log₂(n / 2)
           = (n / 2) · [ log₂ n - 1 ]
           = Ω(n log n)
  ```
- **Consequence for Exam:** No comparison-based sorting algorithm (Merge Sort, Quick Sort, Heap Sort) can ever run in less than `Ω(n log n)` comparisons in the worst case. Merge Sort and Heap Sort are therefore **asymptotically optimal**.

---

## 8. Proving Correctness: Loop Invariants

### 8.1 The Three Mandatory Proof Steps
A **loop invariant** is a mathematical condition that remains true before and after each iteration of a loop:
1. **Initialization:** Invariant is true prior to the very first iteration of the loop (base case).
2. **Maintenance:** If it is true before an iteration, it remains true immediately before the next iteration (inductive step).
3. **Termination:** When the loop terminates, the invariant provides a guarantee that the algorithm has computed the correct output.

### 8.2 Example: Insertion Sort Invariant
- **Invariant Statement:** At the start of each iteration of the outer loop `for j = 2 to n`, the subarray `A[1 .. j-1]` consists of the original elements, but in sorted order.
- **Proof:**
  - *Initialization:* Before `j = 2`, subarray `A[1 .. 1]` contains 1 element, which is trivially sorted. Invariant holds.
  - *Maintenance:* The inner loop shifts elements `A[i] > A[j]` rightwards and inserts `A[j]` in its sorted slot. Now `A[1 .. j]` is sorted. Invariant holds.
  - *Termination:* Loop terminates when `j = n + 1`. Substituting: `A[1 .. n]` is sorted. (Proved)

---

## 9. Strassen's Matrix Multiplication (Overview & Recurrence)
> **Source:** `DAAOA_Question_Bank.docx`, Unit I, Q14(a)

- **Standard Matrix Multiplication:** Multiplies two `n × n` matrices by splitting into four `(n/2) × (n/2)` blocks. Requires **8** recursive submatrix multiplications:
  ```
  T(n) = 8T(n/2) + O(n²)  ==>  Θ(n^(log₂ 8)) = Θ(n³)
  ```
- **Strassen's Discovery:** Reduced the number of recursive multiplications from 8 down to **7** using 10 algebraic combinations (`P1` to `P7`):
  ```
  T(n) = 7T(n/2) + Θ(n²)
  ```
- **Solving by Master Theorem:**
  - `a = 7`, `b = 2`, `f(n) = Θ(n²)`.
  - Critical function: `n^(log₂ 7) ≈ n^2.807`.
  - Since `f(n) = n² = O(n^(2.807 - ε))` where `ε ≈ 0.807`, Case 1 applies:
    ```
    T(n) = Θ(n^(log₂ 7)) ≈ Θ(n^2.81)
    ```
- **Benefit:** For large matrices, `O(n^2.81)` is substantially faster than `O(n³)`.

---

## 10. Traced Worked Numerical: Merge Sort
> **Source:** `DAAOA_Question_Bank.docx`, Unit I, Q15(b)

**Problem:** Sort the array: `[78, 32, 42, 62, 98, 12, 34, 83, 10]`

### Step 1: Divide Tree (Splitting Down to Single Elements)
- Initial list (size 9): `[78, 32, 42, 62, 98 | 12, 34, 83, 10]`
- Left side (size 5): `[78, 32, 42 | 62, 98]`
  - Sub-left (size 3): `[78, 32 | 42]`  ==>  `[78]` and `[32]`
  - Sub-right (size 2): `[62 | 98]`  ==>  `[62]` and `[98]`
- Right side (size 4): `[12, 34 | 83, 10]`
  - Sub-left (size 2): `[12 | 34]`  ==>  `[12]` and `[34]`
  - Sub-right (size 2): `[83 | 10]`  ==>  `[83]` and `[10]`

### Step 2: Conquer & Merge (Bottom-Up)
1. Merge `[78]` and `[32]` → `[32, 78]`
2. Merge `[32, 78]` with `[42]` → `[32, 42, 78]`
3. Merge `[62]` and `[98]` → `[62, 98]`
4. Merge `[32, 42, 78]` with `[62, 98]` → **`[32, 42, 62, 78, 98]`** (Left half sorted)
5. Merge `[12]` and `[34]` → `[12, 34]`
6. Merge `[83]` and `[10]` → `[10, 83]`
7. Merge `[12, 34]` with `[10, 83]` → **`[10, 12, 34, 83]`** (Right half sorted)
8. **Final 2-Way Merge:**
   - Compare 32 vs 10 → take `10`
   - Compare 32 vs 12 → take `12`
   - Compare 32 vs 34 → take `32`
   - Compare 42 vs 34 → take `34`
   - Compare 42 vs 83 → take `42`
   - Compare 62 vs 83 → take `62`
   - Compare 78 vs 83 → take `78`
   - Compare 98 vs 83 → take `83`
   - Flush remaining from left → take `98`
- **Final Output:** `[10, 12, 32, 34, 42, 62, 78, 83, 98]`

---

## 11. Advanced Mathematical Proofs & Discrete Invariants

### 11.1 Harmonic Series Bounding via Integrals
- **Definition:** The $n$-th Harmonic number is $H_n = \sum_{k=1}^n \frac{1}{k} = 1 + \frac{1}{2} + \frac{1}{3} + \dots + \frac{1}{n}$.
- **Monotonicity & Integral Bounding:**
  Because $f(x) = \frac{1}{x}$ is strictly positive and monotonically decreasing for all $x \ge 1$:
  $$\frac{1}{k} \le \int_{k-1}^k \frac{1}{x} \, dx \quad \text{for } k \ge 2$$
  Summing from $k = 2$ to $n$:
  $$\sum_{k=2}^n \frac{1}{k} \le \int_1^n \frac{1}{x} \, dx = \ln n$$
  Adding the initial term $k = 1$:
  $$H_n = 1 + \sum_{k=2}^n \frac{1}{k} \le 1 + \ln n = O(\log n)$$
- **Lower Bound:** Similarly, $\sum_{k=1}^n \frac{1}{k} \ge \int_1^{n+1} \frac{1}{x} \, dx = \ln(n+1) = \Omega(\log n)$.
- **Conclusion:** $H_n = \mathbf{\Theta(\log n)}$.

---

### 11.2 Discrete Invariants: The Bracelet / Necklace Bisection Problem
- **Problem Statement:** A circular bracelet contains $2n$ black and $2n$ white beads. Prove that we can partition the bracelet into two contiguous arcs by making exactly two cuts such that each arc contains exactly $n$ black and $n$ white beads.
- **Proof via Discrete Intermediate Value Theorem (IVT):**
  1. Label the beads along the circular perimeter from $1$ to $4n$.
  2. For each $i \in \{1, \dots, 2n\}$, let $A(i)$ denote the semicircular arc of length $2n$ starting at bead $i$.
  3. Let $W(i)$ be the number of white beads in arc $A(i)$.
  4. The complementary arc is $A(i + 2n)$. Because the total number of white beads is $2n$:
     $$W(i) + W(i + 2n) = 2n \implies W(i + 2n) = 2n - W(i)$$
  5. Define the discrepancy function $\Delta(i) = W(i) - n$.
     Then $\Delta(i + 2n) = W(i + 2n) - n = (2n - W(i)) - n = -\Delta(i)$.
  6. When advancing from arc $i$ to $i+1$, arc $A(i+1)$ drops bead $i$ and acquires bead $i + 2n$.
     Therefore, the white bead count changes by at most $\pm 1$:
     $$|\Delta(i+1) - \Delta(i)| \le 1$$
  7. If $\Delta(1) = 0$, we are done. If $\Delta(1) > 0$, then $\Delta(1 + 2n) = -\Delta(1) < 0$.
  8. Since $\Delta(i)$ transitions from positive to negative in integer steps of at most $\pm 1$, by the **Discrete Intermediate Value Theorem**, there must exist an intermediate index $i^*$ such that:
     $$\Delta(i^*) = 0 \iff W(i^*) = n \quad \text{and} \quad B(i^*) = 2n - n = n$$
  9. Cutting before bead $i^*$ and after bead $i^* + 2n - 1$ yields two contiguous portions, each with exactly $n$ white and $n$ black beads. $\blacksquare$

