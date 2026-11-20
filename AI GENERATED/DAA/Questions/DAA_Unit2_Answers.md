# DAA Unit II: Model Answers & Marking Scheme (Recursion & Divide and Conquer)

> **Sources Used:**
> - `DAA_Question_Bank_Ans_Final.docx` (Unit II)
> - `DAA Endsem Revision (CS3212).pdf` (Part C, Solutions)
> - `BOOKS\Introduction to Algorithms_Cormen_3rd Ed.pdf` (CLRS)

---

## Solution Q1 [4 Marks]

### 1. Statement of Master Theorem (2 Marks)
For a divide-and-conquer recurrence of the form:
```
T(n) = a · T(n/b) + f(n),   where a ≥ 1, b > 1
```
Compare `f(n)` with the critical function `n^(log_b a)`:
1. **Case 1:** If `f(n) = O(n^(log_b a - ε))` for `ε > 0`, then `T(n) = Θ(n^(log_b a))`.
2. **Case 2:** If `f(n) = Θ(n^(log_b a) · (log n)^k)` for `k ≥ 0`, then `T(n) = Θ(n^(log_b a) · (log n)^(k+1))`.
3. **Case 3:** If `f(n) = Ω(n^(log_b a + ε))` for `ε > 0` and `a · f(n/b) ≤ c · f(n)` for `c < 1`, then `T(n) = Θ(f(n))`.

### 2. Solve T(n) = 3T(n/4) + n log n (2 Marks)
- Parameters: `a = 3`, `b = 4`, `f(n) = n log n`.
- Compute critical exponent:
  ```
  n^(log_b a) = n^(log₄ 3) ≈ n^0.793
  ```
- Compare `f(n)` with `n^(log₄ 3)`:
  Since `f(n) = n¹ · log n`, its polynomial degree is `1`.
  Notice `1 > 0.793`. Specifically:
  ```
  f(n) = Ω(n^(0.793 + ε))   where ε ≈ 0.207
  ```
- Check Regularity Condition for Case 3:
  ```
  a · f(n/b) = 3 · (n/4) · log(n/4) = (3/4) · n · [log n - log 4] ≤ (3/4) · n log n
  ```
  For `c = 3/4 < 1`, regularity holds.
- **Conclusion:** By Case 3 of the Master Theorem:
  ```
  T(n) = Θ(f(n)) = Θ(n log n)
  ```

---

## Solution Q2 [4 Marks]

### 1. Josephus Recurrence Relation (1.5 Marks)
For a circle of `n` people with every 2nd person eliminated:
- If `n` is even (`n = 2k`):
  ```
  J(2k) = 2 · J(k) - 1
  ```
- If `n` is odd (`n = 2k + 1`):
  ```
  J(2k + 1) = 2 · J(k) + 1
  ```
- Base Case: `J(1) = 1`.

### 2. Binary Representation Method for J(41) (2.5 Marks)
- Express `n = 41` in the form `n = 2ᵐ + L` where `2ᵐ` is the highest power of 2 strictly `≤ 41`:
  ```
  41 = 32 + 9 = 2⁵ + 9
  ==> m = 5,   L = 9
  ```
- Using the closed-form theorem:
  ```
  J(n) = 2L + 1
  J(41) = 2(9) + 1 = 18 + 1 = 19
  ```
- **Binary Bitwise Verification:**
  - `41` in binary = `101001₂`
  - Perform 1-bit cyclic left rotation (move leading `1` to LSB):
    `1 0 1 0 0 1` → `0 1 0 0 1 1₂`
  - `010011₂ = 16 + 2 + 1 = 19₁₀`.
- **Final Answer:** Survivor position is **19**.

---

## Solution Q3 [4 Marks]

### Problem: Compute 7¹³ mod 11 using Square-and-Multiply
- Exponent `b = 13`.
- Convert `13` to binary: `13 = 8 + 4 + 1 = 1101₂`.
- Bits from Left-to-Right (MSB to LSB): `[1, 1, 0, 1]`.

### Step-by-Step Trace Table (3 Marks)
Initialize `result = 1`. For each bit:
- Always square: `result = (result²) mod 11`
- If bit is 1, also multiply by base: `result = (result · 7) mod 11`

| Step | Bit | Operation | Formula & Modular Math | Current Value |
|:----:|:---:|-----------|------------------------|:-------------:|
| **Init** | - | Start | `result = 1` | 1 |
| **Bit 1** | `1` | Square & Multiply | `(1² · 7) mod 11 = 7 mod 11` | **7** |
| **Bit 2** | `1` | Square & Multiply | `7² mod 11 = 49 mod 11 = 5`<br>`(5 · 7) mod 11 = 35 mod 11 = 2` | **2** |
| **Bit 3** | `0` | Square Only | `2² mod 11 = 4 mod 11` | **4** |
| **Bit 4** | `1` | Square & Multiply | `4² mod 11 = 16 mod 11 = 5`<br>`(5 · 7) mod 11 = 35 mod 11 = 2` | **2** |

### Final Result (1 Mark)
```
7¹³ mod 11 = 2
```

---

## Solution Q4 [4 Marks]

### 1. Definition of Inversion (1 Mark)
Given an array `A[1 .. n]`, an **inversion** is any pair of indices `(i, j)` such that:
```
i < j   and   A[i] > A[j]
```
An array has 0 inversions if sorted, and `n(n - 1) / 2` inversions if reverse-sorted.

### 2. Modified Merge Sort Algorithm (3 Marks)
- Naive double loop takes `O(n²)` time.
- **D&C Modification:**
  1. Recursively count inversions in Left half `A[p .. q]`: `count_left`.
  2. Recursively count inversions in Right half `A[q+1 .. r]`: `count_right`.
  3. Count **Split Inversions** during the two-pointer `Merge()` procedure:
     - Maintain pointer `i` for left subarray and pointer `j` for right subarray.
     - When `A[i] ≤ A[j]`: no inversion; copy `A[i]` to output.
     - When `A[i] > A[j]`: since left subarray is sorted, `A[j]` is smaller than `A[i]` AND **all remaining elements from index `i` to `q`**!
     - Therefore, increment split inversion count by:
       ```
       split_inversions += (q - i + 1)
       ```
     - Copy `A[j]` to output.
- **Recurrence:** `T(n) = 2T(n/2) + O(n) = Θ(n log n)`.

---

## Solution Q5 [4 Marks]

| Feature | Randomized Quickselect | Median-of-Medians (Deterministic) |
|---------|------------------------|-----------------------------------|
| **Pivot Selection** | Chosen uniformly at random from subarray. | Recursively finds median of medians of 5-element groups. |
| **Expected Time** | `Θ(n)` (fast constant factors). | `Θ(n)`. |
| **Worst-Case Time** | `O(n²)` (if random pivot is always extreme). | **`Θ(n)` guaranteed** worst-case bound. |
| **Auxiliary Space** | `O(1)` iterative / `O(log n)` stack. | `O(log n)` stack due to recursive pivot calls. |
| **Practical Use** | Industry standard (`std::nth_element` in C++). | Theoretical significance; high constant overhead in practice. |

---

## Solution Q6 [4 Marks]

### 1. Union by Rank Heuristic (1.5 Marks)
- Each set root stores a `rank` (an upper bound on tree height).
- When executing `Union(x, y)`:
  - Find roots `rx = Find(x)` and `ry = Find(y)`.
  - Attach the root with smaller rank under the root with larger rank.
  - If ranks are equal, pick one as root and increment its rank by 1.
- *Benefit:* Prevents trees from degenerating into linear chains; guarantees maximum tree height is `O(log n)`.

### 2. Path Compression (Collapsing Rule) (1.5 Marks)
- During `Find(x)`:
  - Traverse up pointers to find set root `r`.
  - While unwinding recursion, directly reparent `x` and **every node on the path** directly to root `r`.
- *Benefit:* Tree becomes almost completely flat for future operations.

### 3. Combined Time Complexity (1 Mark)
When both Union-by-Rank and Path Compression are used, any sequence of `m` operations on `n` elements runs in:
```
T = O(m · α(n))
```
Where `α(n)` is the **Inverse Ackermann Function**. Since `α(n) ≤ 4` for all practical values of `n`, each operation takes **virtually constant amortized time `O(1)`**.

---

## Solution Q7 [10 Marks]

### Part a: Karatsuba's Algorithm & Recurrence (5 Marks)
- **Concept (2 Marks):** To multiply two `n`-digit numbers `X` and `Y`, split into halves of size `n/2`:
  ```
  X = a · 10^(n/2) + b,   Y = c · 10^(n/2) + d
  X · Y = a·c · 10ⁿ + (a·d + b·c) · 10^(n/2) + b·d
  ```
  Standard algebra needs 4 multiplications (`ac, ad, bc, bd`).
- **Karatsuba's Trick (2 Marks):**
  Compute only **3 multiplications**:
  1. `P1 = a · c`
  2. `P2 = b · d`
  3. `P3 = (a + b) · (c + d) = a·c + a·d + b·c + b·d`
  Observe that the middle term is:
  ```
  a·d + b·c = P3 - P1 - P2
  ```
- **Recurrence & Master Theorem (1 Mark):**
  ```
  T(n) = 3 · T(n/2) + O(n)
  ```
  `a = 3, b = 2, f(n) = O(n)`. Critical: `n^(log₂ 3) ≈ n^1.585`.
  By Case 1 of Master Theorem:
  ```
  T(n) = Θ(n^(log₂ 3)) ≈ Θ(n^1.585)
  ```

### Part b: Multiply 1234 × 5678 (5 Marks)
- `n = 4, n/2 = 2`.
- `X = 1234` → `a = 12`, `b = 34`.
- `Y = 5678` → `c = 56`, `d = 78`.

**Step 1: Compute the 3 sub-products (3 Marks)**
1. `P1 = a · c = 12 × 56 = 672`
2. `P2 = b · d = 34 × 78 = 2652`
3. `P3 = (a + b) · (c + d) = (12 + 34) × (56 + 78) = 46 × 134 = 6164`

**Step 2: Compute middle term (1 Mark)**
```
Middle = P3 - P1 - P2 = 6164 - 672 - 2652 = 2840
```

**Step 3: Combine with base powers (1 Mark)**
```
Product = P1 · 10⁴ + Middle · 10² + P2
        = 672 · 10000 + 2840 · 100 + 2652
        = 6,720,000 + 284,000 + 2,652
        = 7,006,652
```
*(Exact verification: 1234 × 5678 = 7,006,652).*

---

## Solution Q8 [10 Marks]

### Part a: Closest Pair 2D & 7-Point Strip Rationale (6 Marks)
- **Algorithm (3 Marks):**
  1. Pre-sort all points by x-coordinate (`Px`) and y-coordinate (`Py`) in `O(n log n)`.
  2. Draw vertical midline `x = mid` dividing points into `Left` and `Right` of size `n/2`.
  3. Recursively find closest pair distance in Left (`δ_L`) and Right (`δ_R`). Set `δ = min(δ_L, δ_R)`.
  4. Create strip array of points whose x-coordinate is within `[mid - δ, mid + δ]`, sorted by y.
  5. For each point `p` in strip, compare distance to subsequent points in strip within y-distance `δ`.
- **7-Point Comparison Rationale (3 Marks):**
  - Consider a rectangle of width `2δ` and height `δ` centered on the vertical line.
  - This rectangle is divided into two squares of dimension `δ × δ` (left and right of midline).
  - Inside each `δ × δ` square, all points must be at distance `≥ δ` from each other (by definition of `δ = min(δ_L, δ_R)`).
  - By geometric packing, each `δ × δ` square can contain **at most 4 points** (at the 4 corners).
  - Therefore, the entire `2δ × δ` rectangle can contain at most `4 + 4 = 8` points.
  - Excluding point `p` itself, `p` can have **at most 7 other points** in this region. Comparing against 7 neighbors is sufficient!

### Part b: Recurrence & Complexity (4 Marks)
- Pre-sorting takes `O(n log n)` once.
- Recursive division: 2 subproblems of size `n/2`.
- Strip filtering and linear scanning against at most 7 neighbors: `7 · n = O(n)` time.
```
T(n) = 2T(n/2) + O(n)
```
- By Case 2 of Master Theorem (`a = 2, b = 2, f(n) = n`):
```
T(n) = Θ(n log n)
```

---

## Solution Q9 [8 Marks]

### Part a: Quick Sort Derivations (5 Marks)
- **Best Case (1.5 Marks):** Pivot always divides array into two equal halves of size `n/2`:
  ```
  T(n) = 2T(n/2) + c · n  ==>  Θ(n log n)
  ```
- **Worst Case (1.5 Marks):** Pivot is always minimum or maximum (e.g., sorted array with first element pivot):
  ```
  T(n) = T(n - 1) + c · n = c · [n(n + 1)/2]  ==>  Θ(n²)
  ```
- **Average Case & Randomization (2 Marks):**
  - Standard Quick Sort has input-dependent worst case.
  - In **Randomized Quick Sort**, the pivot index is chosen uniformly at random from `[low .. high]`.
  - No adversarial fixed input can force worst-case behavior.
  - The expected number of comparisons across any input of size `n` is `2n ln n = Θ(n log n)`.

### Part b: Recommendation Justifications (3 Marks)
1. **Sorted Data:** Recommend **Merge Sort** (`Θ(n log n)` guaranteed; standard Quick Sort degrades to catastrophic `O(n²)`).
2. **Severe Memory Limits:** Recommend **Quick Sort** (operates in-place with `O(log n)` call stack; Merge Sort requires `O(n)` extra array).
3. **Singly Linked List:** Recommend **Merge Sort** (linked lists can be split in `O(n)` and merged in-place in `O(1)` auxiliary space without random access!).

---

## Solution Q10 [8 Marks]

### Part a: Tower of Hanoi Proof (5 Marks)
- **Algorithm (2 Marks):**
  ```text
  Hanoi(n, A, C, B):
    if n == 1:
      Move disk 1 from A to C
      return
    Hanoi(n - 1, A, B, C)
    Move disk n from A to C
    Hanoi(n - 1, B, C, A)
  ```
- **Recurrence & Solution (3 Marks):**
  ```
  T(n) = 2T(n - 1) + 1,   with T(1) = 1
  ```
  Unfolding:
  ```
  T(n) = 2[2T(n - 2) + 1] + 1 = 4T(n - 2) + 2 + 1
       = 2ᵏ · T(n - k) + Sum(i=0 to k-1) [2ⁱ]
  ```
  Set `k = n - 1`:
  ```
  T(n) = 2^(n-1) · 1 + (2^(n-1) - 1) = 2 · 2^(n-1) - 1 = 2ⁿ - 1
  ```
  Number of moves is exactly `2ⁿ - 1` → `Θ(2ⁿ)`.

### Part b: Boyer-Moore Majority Vote (3 Marks)
- **Trace on `[2, 2, 1, 1, 1, 2, 2]`:**
  - Init: `candidate = None, count = 0`.
  - Item 2: `count = 0` → `candidate = 2, count = 1`.
  - Item 2: `2 == candidate` → `count = 2`.
  - Item 1: `1 != candidate` → `count = 1`.
  - Item 1: `1 != candidate` → `count = 0`.
  - Item 1: `count = 0` → `candidate = 1, count = 1`.
  - Item 2: `2 != candidate` → `count = 0`.
  - Item 2: `count = 0` → `candidate = 2, count = 1`.
  - Final candidate = **`2`**.
  - Verification pass: count of 2 is `4 / 7 > 0.5`. **Majority Element is 2**.
- **Complexities:** Time = `O(n)`, Space = `O(1)`.
