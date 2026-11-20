# DAA Unit I: One-Page Exam Cheat Sheet
**Course:** CS3212 (Design and Analysis of Algorithms) | **Unit I:** Complexity Analysis

> **Source Files Used:**
> - DAA SYLLABUS.pdf (Unit I)
> - DAA Endsem Revision (CS3212).pdf (pp. 1, 5)
> - DAAOA_Question_Bank.docx (Unit I)
> - DAA_Question_Bank_Ans_Final.docx (Unit I)

---

## 1. Asymptotic Notations Quick Reference

| Notation | Name | Mathematical Condition | Boundary Type | Exam Meaning |
|:--------:|:----:|:----------------------:|:-------------:|:-------------|
| **O(g(n))** | Big-O | `0 ≤ f(n) ≤ c · g(n)` for all `n ≥ n0` | Upper Bound | Worst-case running time ceiling (guaranteed maximum) |
| **Ω(g(n))** | Big-Omega | `0 ≤ c · g(n) ≤ f(n)` for all `n ≥ n0` | Lower Bound | Best-case performance floor (guaranteed minimum) |
| **Θ(g(n))** | Big-Theta | `c1 · g(n) ≤ f(n) ≤ c2 · g(n)` for all `n ≥ n0` | Tight Bound | Exact growth rate (f is in BOTH O and Ω) |
| **o(g(n))** | Little-o | `0 ≤ f(n) < c · g(n)` for all `c > 0, n ≥ n0` | Strict Upper | Ratio `f(n) / g(n) → 0` as `n → ∞` |
| **ω(g(n))** | Little-omega | `0 ≤ c · g(n) < f(n)` for all `c > 0, n ≥ n0` | Strict Lower | Ratio `f(n) / g(n) → ∞` as `n → ∞` |

### Growth Order Ladder (Memorize strictly for exams):
`1 < log n < √n < n < n log n < n² < n³ < 2ⁿ < n! < nⁿ`

---

## 2. Master Theorem Rules: T(n) = a·T(n/b) + f(n)
*Valid when:* `a ≥ 1, b > 1, f(n) > 0`. Compare `f(n)` with `n^(log_b a)`:

1. **Case 1 (f(n) is smaller):** If `f(n) = O(n^(log_b a - ε))` for some `ε > 0`:
   → **`T(n) = Θ(n^(log_b a))`**
2. **Case 2 (f(n) is equal):** If `f(n) = Θ(n^(log_b a) · (log n)^k)` for `k ≥ 0`:
   → **`T(n) = Θ(n^(log_b a) · (log n)^(k+1))`**  *(When k = 0, T(n) = Θ(n^(log_b a) · log n))*
3. **Case 3 (f(n) is larger):** If `f(n) = Ω(n^(log_b a + ε))` and regularity `a·f(n/b) ≤ c·f(n)` holds (`c < 1`):
   → **`T(n) = Θ(f(n))`**

---

## 3. Algorithm Complexity & Recurrence Master Table

| Algorithm | Recurrence Equation | Best Case | Worst Case | Average Case | Auxiliary Space |
|-----------|---------------------|:---------:|:----------:|:------------:|:---------------:|
| **Linear Search** | `T(n) = T(n-1) + 1` | `O(1)` | `O(n)` | `O(n)` | `O(1)` |
| **Binary Search** | `T(n) = T(n/2) + 1` | `O(1)` | `O(log n)` | `O(log n)` | `O(1)` iter / `O(log n)` rec |
| **Quick Sort** | `T(n) = T(n-1) + n` (worst) | `O(n log n)` | `O(n²)` | `O(n log n)` | `O(log n)` avg / `O(n)` worst |
| **Merge Sort** | `T(n) = 2T(n/2) + n` | `O(n log n)` | `O(n log n)` | `O(n log n)` | `O(n)` |
| **Strassen's Matrix Mult** | `T(n) = 7T(n/2) + O(n²)` | `O(n^2.81)` | `O(n^2.81)` | `O(n^2.81)` | `O(n²)` stack |
| **BFS Traversal** | Queue based | `O(V + E)` | `O(V + E)` | `O(V + E)` | `O(V)` |
| **DFS Traversal** | Stack / recursive | `O(V + E)` | `O(V + E)` | `O(V + E)` | `O(V)` |
| **Topological Sort** | DFS finish / Kahn's algorithm | `O(V + E)` | `O(V + E)` | `O(V + E)` | `O(V)` |

---

## 4. Key Mathematical Derivations

- **Sum of first n integers:**
  `1 + 2 + 3 + ... + n = n(n + 1) / 2 = Θ(n²)`
- **Sum of geometric series (powers of 2):**
  `1 + 2 + 4 + ... + 2ᵏ = 2^(k+1) - 1`
- **Stirling's approximation:**
  `log₂(n!) = log₂(1) + log₂(2) + ... + log₂(n) = Θ(n log n)`
- **Adversary Sorting Lower Bound Derivation:**
  1. A comparison sort is a binary decision tree.
  2. For `n` elements, there are `n!` permutations, so leaves `L ≥ n!`.
  3. A binary tree of height `h` has at most `2ʰ` leaves: `2ʰ ≥ L ≥ n!`.
  4. Taking log: `h ≥ log₂(n!) = Ω(n log n)`.
  5. Therefore, no comparison sort can run faster than `Ω(n log n)` in the worst case.
- **Loop Invariant 3 Steps:**
  1. **Initialization:** True before the loop starts (base case).
  2. **Maintenance:** If true before iteration k, remains true before iteration k+1 (inductive step).
  3. **Termination:** When the loop exits, provides a useful property that proves algorithm correctness.

---

## 5. Donald Knuth's 5 Criteria of an Algorithm
1. **Input:** Zero or more externally supplied quantities.
2. **Output:** At least one quantity produced.
3. **Definiteness:** Every step is completely clear, precise, and unambiguous.
4. **Finiteness:** Must terminate after a finite number of operations for all inputs.
5. **Effectiveness:** Every operation is basic enough to be carried out manually with pencil and paper.

---

## 6. Common Traps to Avoid in Exam
- **Trap 1:** Writing `O(n log n)` for worst-case QuickSort. *(Worst case is O(n²) when the input is already sorted!)*
- **Trap 2:** Confusing Big-O with worst case. *(Big-O is a mathematical upper bound; both best and worst cases have Big-O representations).*
- **Trap 3:** Applying Master Theorem when `b ≤ 1` or recurrence is non-polynomial like `T(n) = 2T(n/2) + n!`.
- **Trap 4:** Proving `Θ` with only one constant. *(You MUST show BOTH c1 and c2 with a valid n0).*
