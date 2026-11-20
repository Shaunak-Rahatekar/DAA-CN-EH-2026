# DAA Unit II: Recursion and Divide & Conquer

> **Sources Used:**
> - `D:\2026 EXAM\DAA\DAA SYLLABUS.pdf` (CS3212, Unit II)
> - `D:\2026 EXAM\DAA\DAA Endsem Revision (CS3212).pdf` (Part A, Unit II)
> - `D:\2026 EXAM\DAA\DAAOA_Question_Bank.docx` (Unit II)
> - `D:\2026 EXAM\DAA\DAA_Question_Bank_Ans_Final.docx` (Unit II Model Answers)
> - `D:\2026 EXAM\DAA\BOOKS\Introduction to Algorithms_Cormen_3rd Ed.pdf` (CLRS Chapters 4, 9, 21, 33)
> - `D:\2026 EXAM\DAA\BOOKS\Algorithm Design - John Kleinberg - Éva Tardos.pdf` (Chapters 5, 13)

---

## 1. The Divide-and-Conquer Paradigm

### 1.1 The Three Core Steps
Divide-and-Conquer is an algorithmic paradigm based on multi-branched recursion:
1. **Divide:** Break the original problem into two or more smaller, disjoint subproblems of the same type.
2. **Conquer:** Solve each subproblem recursively. If subproblems are small enough (base case), solve them directly.
3. **Combine (Merge):** Combine the solutions of the subproblems into a solution for the original problem.

> **Everyday Analogy:** Imagine organizing an exam library with 1,000 messy books. Instead of 1 person checking all 1,000 books, you divide them into two stacks of 500, hand them to two assistants to sort recursively (Divide & Conquer), and then cleanly zipper-merge the two sorted stacks together (Combine).

### 1.2 Mathematical Formulation of Recurrence
If a problem of size `n` is divided into `a` subproblems, each of size `n/b`, and the divide/combine step takes time `D(n) + C(n) = f(n)`:
```
T(n) = a · T(n/b) + f(n),   with base case T(1) = Θ(1)
```
- `a ≥ 1`: number of subproblems generated in each recursive step.
- `b > 1`: factor by which the subproblem size decreases.
- `f(n)`: work done outside recursive calls (dividing input + combining outputs).

---

## 2. Master Theorem & Recurrence Solving

### 2.1 The Master Theorem for Divide-and-Conquer
For recurrences of the form `T(n) = a · T(n/b) + f(n)` where `a ≥ 1`, `b > 1`, compare `f(n)` with the critical boundary function `n^(log_b a)`:

| Case | Condition on f(n) | Solution T(n) | Practical Exam Intuition |
|:----:|:-------------------|:-------------:|:-------------------------|
| **Case 1** | `f(n) = O(n^(log_b a - ε))` for `ε > 0` | **`Θ(n^(log_b a))`** | Tree leaves dominate total cost (leaf-heavy). |
| **Case 2** | `f(n) = Θ(n^(log_b a) · (log n)^k)` for `k ≥ 0` | **`Θ(n^(log_b a) · (log n)^(k+1))`** | Work is evenly distributed across all tree levels. |
| **Case 3** | `f(n) = Ω(n^(log_b a + ε))` and `a·f(n/b) ≤ c·f(n)` (`c < 1`) | **`Θ(f(n))`** | Root / combine step dominates total cost (root-heavy). |

### 2.2 Standard Recurrences Derived by Master Theorem
1. **Binary Search:** `T(n) = T(n/2) + 1`
   - `a = 1, b = 2, f(n) = 1 = n⁰`. Critical exponent: `n^(log₂ 1) = n⁰ = 1`.
   - Case 2 applies (`k = 0`) → **`T(n) = Θ(log n)`**.
2. **Merge Sort:** `T(n) = 2T(n/2) + n`
   - `a = 2, b = 2, f(n) = n¹`. Critical: `n^(log₂ 2) = n¹`.
   - Case 2 applies (`k = 0`) → **`T(n) = Θ(n log n)`**.
3. **Karatsuba Multiplication:** `T(n) = 3T(n/2) + n`
   - `a = 3, b = 2, f(n) = n¹`. Critical: `n^(log₂ 3) ≈ n^1.585`.
   - Since `n¹ = O(n^(1.585 - 0.585))`, Case 1 applies → **`T(n) = Θ(n^1.585)`**.
4. **Strassen's Matrix Multiplication:** `T(n) = 7T(n/2) + n²`
   - `a = 7, b = 2, f(n) = n²`. Critical: `n^(log₂ 7) ≈ n^2.807`.
   - Case 1 applies → **`T(n) = Θ(n^2.81)`**.

---

## 3. Combinatorial Enumeration & Classical Recursive Puzzles

### 3.1 Tower of Hanoi
- **Problem:** Move `n` disks from source peg `A` to destination peg `C` using auxiliary peg `B`, never placing a larger disk on a smaller disk.
- **Algorithm Strategy:**
  1. Move top `n - 1` disks from `A` to `B` (using `C`).
  2. Move largest disk `n` directly from `A` to `C`.
  3. Move `n - 1` disks from `B` to `C` (using `A`).
- **Recurrence:**
  ```
  T(n) = 2T(n - 1) + 1,   with T(1) = 1
  ```
- **Solution by Unfolding:**
  ```
  T(n) = 2[2T(n - 2) + 1] + 1 = 4T(n - 2) + 2 + 1
       = 2ᵏ · T(n - k) + (2^(k-1) + ... + 2 + 1)
  ```
  Set `n - k = 1` → `k = n - 1`:
  ```
  T(n) = 2^(n-1) · 1 + (2^(n-1) - 1) = 2ⁿ - 1
  ```
- **Complexity:** Exactly `2ⁿ - 1` moves. Time complexity = `Θ(2ⁿ)`. Space = `O(n)` call stack.

---

### 3.2 Josephus Problem
- **Problem:** `n` people stand in a circle numbered `1` to `n`. Starting from person 1, every 2nd person is eliminated until only 1 survivor remains. Find survivor position `J(n)`.
- **Recurrence Formulation:**
  - If `n` is even (`n = 2k`): All even positions (2, 4, ..., 2k) are eliminated in the first round. Person 1 is next. It reduces to a circle of size `k`, but with new labels:
    ```
    J(2k) = 2 · J(k) - 1
    ```
  - If `n` is odd (`n = 2k + 1`): After 2, 4, ..., 2k are eliminated, person 1 is eliminated next, leaving person 3 next in turn:
    ```
    J(2k + 1) = 2 · J(k) + 1
    ```
  - Base Case: `J(1) = 1`.
- **The Binary Representation Trick (Exam Favorite):**
  Write `n` in binary as `n = 1 b_k b_(k-1) ... b_1 b_0` (where `n = 2ᵐ + L` with `0 ≤ L < 2ᵐ`).
  The survivor is:
  ```
  J(n) = 2L + 1
  ```
  *Bitwise Rule:* Cyclic one-bit left rotation of binary `n`! (Move the leading `1` bit to the end).
  - *Example from Revision Guide:* Find `J(41)`:
    - `41 = 32 + 9 = 2⁵ + 9`  →  `m = 5, L = 9`.
    - `J(41) = 2(9) + 1 = 19`.
    - Binary verification: `41 = 101001₂`. Rotate left: `010011₂ = 19₁₀`!

---

## 4. Advanced Divide-and-Conquer Applications

### 4.1 Counting Inversions in an Array
- **Definition:** An inversion is a pair of indices `(i, j)` such that `i < j` but `A[i] > A[j]`. Measures how unsorted an array is.
- **Algorithm (Modified Merge Sort):**
  1. Split array into Left and Right halves.
  2. Recursively count inversions in Left half: `inv_left`.
  3. Recursively count inversions in Right half: `inv_right`.
  4. Count split inversions during the `Merge` step: When element `A[j]` from the right subarray is copied before `A[i]` from the left subarray, it is inverted with **all remaining elements** in the left subarray:
     ```
     inversions += (mid - i + 1)
     ```
- **Complexity:** `T(n) = 2T(n/2) + O(n) = Θ(n log n)`. (Much faster than naive `O(n²)`).

---

### 4.2 Finding a Majority Element
- **Definition:** An element that appears strictly more than `n/2` times in an array of size `n`.
- **Methods:**
  1. **Divide and Conquer:**
     - Split array into two halves. Find majority element of Left half (`m_L`) and Right half (`m_R`).
     - If `m_L == m_R`, that is the majority element.
     - If they differ, count occurrences of both candidates in full array in linear time.
     - Recurrence: `T(n) = 2T(n/2) + O(n) = Θ(n log n)`.
  2. **Boyer-Moore Voting Algorithm [Linear Time O(n), O(1) Space]:**
     - Maintain a `candidate` and a `count = 0`.
     - Scan through array: if `count == 0`, set `candidate = x`. If `x == candidate`, `count++`, else `count--`.
     - Second pass: verify if candidate appears `> n/2` times.

---

### 4.3 Order Statistics (Finding the k-th Smallest Element)
- **Goal:** Find the `k`-th smallest element in an unsorted array of size `n`. (Median corresponds to `k = ⌈n/2⌉`).

#### Approach A: Randomized Quickselect (Expected Linear Time)
- **Strategy:** Like Quick Sort, choose a random pivot and partition array into elements `< pivot` (size `L`) and `> pivot`.
  - If `k == L + 1`: pivot is the `k`-th smallest element.
  - If `k ≤ L`: recurse ONLY on left subarray (throw away right).
  - If `k > L + 1`: recurse ONLY on right subarray for rank `k - (L + 1)`.
- **Complexity:**
  - Expected Time: `T(n) = T(n/2) + O(n) = Θ(n)`.
  - Worst Case: `O(n²)` (if pivot is consistently worst element).
  - Auxiliary Space: `O(1)`.

#### Approach B: Deterministic Median-of-Medians (Guaranteed O(n) Worst Case)
- **BLUM, FLOYD, PRATT, RIVEST, TARJAN Algorithm:**
  1. Group `n` elements into blocks of **5** elements each.
  2. Find median of each 5-element group by insertion sort (cost: `O(1)` per group, total `O(n)`).
  3. Recursively find the median of these `⌈n/5⌉` medians. Let this be pivot `x`.
  4. Partition original array around `x`.
  5. Recurse on the side containing rank `k`.
- **Why size 5?** At least half of the `⌈n/5⌉` medians are `≥ x`. Each such median has at least 3 elements `≥ x`. Thus, at least `3(1/2 · n/5) = 3n/10` elements are `≥ x`, guaranteeing that the subproblem size never exceeds `7n/10`.
- **Recurrence:**
  ```
  T(n) ≤ T(n/5) + T(7n/10) + c · n
  ```
  Since `1/5 + 7/10 = 9/10 < 1`:
  ```
  T(n) = Θ(n)   strictly in the WORST case!
  ```

---

### 4.4 Closest Pair of Points in 2D Plane
- **Problem:** Given `n` points in a plane, find two points with the smallest Euclidean distance `d(p1, p2) = √((x1 - x2)² + (y1 - y2)²)`.
- **Naive Algorithm:** Check all pairs → `O(n²)`.
- **Divide-and-Conquer Algorithm (O(n log n)):**
  1. **Pre-sort:** Sort points by x-coordinate (`Px`) and y-coordinate (`Py`) in `O(n log n)`.
  2. **Divide:** Draw vertical dividing line `x = mid` splitting points into `Left` and `Right` (size `n/2` each).
  3. **Conquer:** Recursively compute closest distance in Left (`δ_L`) and Right (`δ_R`). Set `δ = min(δ_L, δ_R)`.
  4. **Combine (Strip Check):**
     - The closest pair could have one point in Left and one in Right. Such points must lie inside a vertical strip of width `2δ` centered at `x = mid`.
     - Filter points within `[mid - δ, mid + δ]`, ordered by y-coordinate.
     - **Geometric Key Fact:** For each point in the strip, we only need to compare it with at most the **next 7 points** sorted by y! (Because a `δ × 2δ` rectangle can contain at most 8 points with pairwise distance `≥ δ`).
- **Recurrence:**
  ```
  T(n) = 2T(n/2) + O(n) = Θ(n log n)
  ```

---

## 5. Efficient Integer Arithmetic

### 5.1 Euclid's Greatest Common Divisor (GCD) Algorithm
- **Lemma:** `gcd(a, b) = gcd(b, a mod b)`, with base case `gcd(a, 0) = a`.
- **Algorithm:**
  ```text
  EuclidGCD(a, b):
    while b != 0:
      r = a mod b
      a = b
      b = r
    return a
  ```
- **Time Complexity:** Lamé’s Theorem states that the worst case occurs when `a` and `b` are consecutive Fibonacci numbers. Number of steps is at most `5 · (number of decimal digits of b)` → **`O(log(min(a, b)))`**.

---

### 5.2 Fast Modular Exponentiation (Square-and-Multiply)
- **Problem:** Compute `aᵇ mod m` where `b` is a very large integer.
- **Naive Method:** Multiply `a` by itself `b` times → `O(b)` multiplications (intractable for 1024-bit cryptography keys).
- **Square-and-Multiply Property:**
  ```
  aᵇ = (a^(b/2))²       if b is even
  aᵇ = a · (a^((b-1)/2))²  if b is odd
  ```
- **Algorithm:**
  ```text
  ModExp(a, b, m):
    result = 1
    a = a mod m
    while b > 0:
      if b % 2 == 1:
        result = (result * a) mod m
      a = (a * a) mod m
      b = b // 2
    return result
  ```
- **Exam Worked Example:** Compute `7¹³ mod 11`:
  - Binary representation of exponent `13 = 1101₂` (bits from LSB to MSB: 1, 0, 1, 1).
  - Iteration 1 (bit 1): `result = (1 · 7) mod 11 = 7`; `base = 7² mod 11 = 49 mod 11 = 5`.
  - Iteration 2 (bit 0): `result = 7` (unchanged); `base = 5² mod 11 = 25 mod 11 = 3`.
  - Iteration 3 (bit 1): `result = (7 · 3) mod 11 = 21 mod 11 = 10`; `base = 3² mod 11 = 9`.
  - Iteration 4 (bit 1): `result = (10 · 9) mod 11 = 90 mod 11 = 2`.
  - **Final Result:** `7¹³ mod 11 = 2`.
- **Complexity:** Only `O(log b)` multiplications.

---

### 5.3 Karatsuba Integer Multiplication
- **Problem:** Multiply two `n`-digit integers `X` and `Y`.
- **Standard Grade-School Method:** Multiplies each digit of `X` by each digit of `Y` → `O(n²)` operations.
- **Karatsuba's Decomposition:**
  Split `X` and `Y` into high and low `n/2` digits:
  ```
  X = a · 10^(n/2) + b
  Y = c · 10^(n/2) + d
  ```
  Standard product:
  ```
  X · Y = (a · c) · 10ⁿ + (a · d + b · c) · 10^(n/2) + (b · d)
  ```
  Notice that `a·d + b·c` requires 2 multiplications. Combined with `a·c` and `b·d`, that's 4 multiplications:
  `T(n) = 4T(n/2) + O(n) = O(n²)`. No improvement!

- **Karatsuba's Algebraic Trick (Reduces 4 multiplications to 3):**
  Compute:
  1. `P1 = a · c`
  2. `P2 = b · d`
  3. `P3 = (a + b) · (c + d)`
  Then notice:
  ```
  a · d + b · c = P3 - P1 - P2
  ```
  Hence:
  ```
  X · Y = P1 · 10ⁿ + (P3 - P1 - P2) · 10^(n/2) + P2
  ```
- **Recurrence:**
  ```
  T(n) = 3T(n/2) + O(n)
  ```
- **Complexity:** By Master Theorem, `a = 3, b = 2` → **`T(n) = Θ(n^(log₂ 3)) ≈ Θ(n^1.585)`**.

#### Worked Numerical: Multiply 1234 × 5678
> **Source:** `DAA Endsem Revision (CS3212).pdf`, Part C, Q20
- `n = 4`, `n/2 = 2`.
- `X = 1234`  →  `a = 12`, `b = 34`.
- `Y = 5678`  →  `c = 56`, `d = 78`.
- Compute 3 sub-products:
  1. `P1 = a · c = 12 × 56 = 672`
  2. `P2 = b · d = 34 × 78 = 2652`
  3. `P3 = (a + b) · (c + d) = (12 + 34) × (56 + 78) = 46 × 134 = 6164`
- Middle term: `P3 - P1 - P2 = 6164 - 672 - 2652 = 2840`.
- Combine:
  ```
  X · Y = P1 · 10⁴ + (P3 - P1 - P2) · 10² + P2
        = 672 · 10000 + 2840 · 100 + 2652
        = 6720000 + 284000 + 2652
        = 7006652
  ```
  *(Check: 1234 × 5678 = 7,006,652. Exactly correct!)*

---

## 6. Disjoint Set Union (DSU) & Set Operations
> **Source:** `DAAOA_Question_Bank.docx`, Unit II, Q1 to Q10, Q16

### 6.1 Disjoint Set Data Structure
Maintains a collection of pairwise disjoint, dynamic sets `S = {S1, S2, ..., Sk}`. Each set is identified by a **representative element** (the root of the set tree).
- **Core Operations:**
  1. `MakeSet(x)`: Creates a new set containing single element `x`.
  2. `Find(x)`: Returns the representative (root) of the set containing `x`.
  3. `Union(x, y)`: Merges the set containing `x` with the set containing `y`.

### 6.2 Optimizations for High Performance

#### 1. Weighted Union (Union by Rank / Size)
- *Naive Union:* Attaching an arbitrary root under another root can create a degenerate linear tree of depth `n`, making `Find` take `O(n)`.
- *Optimization Rule:* Always attach the tree with smaller depth (or smaller size) under the root of the tree with greater depth.
- *Result:* Keeps tree height bounded by `O(log n)`.

#### 2. Collapsing Rule (Path Compression)
- *Optimization Rule:* During `Find(x)`, traverse up to find root `r`, and on the way down, update the parent pointer of `x` and **every ancestor node visited** directly to `r`.
- *Result:* Flattens the tree so subsequent queries take near `O(1)` time.

### 6.3 Overall Complexity
When **both** Union-by-Rank and Path Compression are used, any sequence of `m` operations on `n` elements takes:
```
Time Complexity = O(m · α(n))
```
Where `α(n)` is the **Inverse Ackermann Function**. For all practical inputs in the universe (`n < 10⁸⁰`), `α(n) ≤ 4`. Thus, DSU runs in **virtually constant amortized time `O(1)`** per operation!
- **Primary Applications:** Kruskal's Minimum Spanning Tree (cycle detection), connected components in dynamic graphs.
