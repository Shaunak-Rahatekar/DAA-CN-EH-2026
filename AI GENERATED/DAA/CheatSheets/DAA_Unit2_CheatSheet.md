# DAA Unit II: One-Page Exam Cheat Sheet
**Course:** CS3212 (Design and Analysis of Algorithms) | **Unit II:** Recursion & Divide and Conquer

> **Source Files Used:**
> - `DAA SYLLABUS.pdf` (Unit II)
> - `DAA Endsem Revision (CS3212).pdf` (pp. 1-2, 5)
> - `DAAOA_Question_Bank.docx` (Unit II)
> - `DAA_Question_Bank_Ans_Final.docx` (Unit II)

---

## 1. Master Recurrence Quick Reference: T(n) = a·T(n/b) + f(n)

| Algorithm | Recurrence Equation | a | b | Critical n^(log_b a) | f(n) | Master Case | Asymptotic Time T(n) |
|-----------|---------------------|:-:|:-:|:--------------------:|:----:|:-----------:|:--------------------:|
| **Binary Search** | `T(n) = T(n/2) + 1` | 1 | 2 | `n⁰ = 1` | 1 | Case 2 (`k=0`) | **`Θ(log n)`** |
| **Merge Sort** | `T(n) = 2T(n/2) + n` | 2 | 2 | `n¹ = n` | `n` | Case 2 (`k=0`) | **`Θ(n log n)`** |
| **Karatsuba Mult** | `T(n) = 3T(n/2) + n` | 3 | 2 | `n^(log₂ 3) ≈ n^1.585` | `n` | Case 1 | **`Θ(n^1.585)`** |
| **Strassen Mult** | `T(n) = 7T(n/2) + n²` | 7 | 2 | `n^(log₂ 7) ≈ n^2.807` | `n²` | Case 1 | **`Θ(n^2.81)`** |
| **Closest Pair 2D** | `T(n) = 2T(n/2) + n` | 2 | 2 | `n¹ = n` | `n` | Case 2 (`k=0`) | **`Θ(n log n)`** |
| **Counting Inversions** | `T(n) = 2T(n/2) + n` | 2 | 2 | `n¹ = n` | `n` | Case 2 (`k=0`) | **`Θ(n log n)`** |
| **Median-of-Medians** | `T(n) ≤ T(n/5) + T(7n/10) + n` | - | - | - | `n` | Induction | **`Θ(n)`** worst-case |

---

## 2. Key Algorithms: Core Idea & Complexity in 1 Line

- **Tower of Hanoi:** Move `n-1` to auxiliary, move largest, move `n-1` to target. Moves = `2ⁿ - 1`. Complexity: `Θ(2ⁿ)`.
- **Josephus Problem:** Survivor `J(2k) = 2J(k) - 1`, `J(2k+1) = 2J(k) + 1`. Binary trick: `J(2ᵐ + L) = 2L + 1` (cyclic 1-bit left rotate).
- **Inversion Count:** Modified Merge Sort. In `Merge()`, when taking right element before left element `A[i]`, add `(mid - i + 1)`. Time: `Θ(n log n)`.
- **Quickselect (Order Stats):** Partition around random pivot, recurse on ONE half only. Expected: `Θ(n)`, Worst: `O(n²)`.
- **Median of Medians:** Group by 5, find medians, pick median-of-medians as pivot. Guarantees `≥ 3n/10` discard. Worst-case: `Θ(n)`.
- **Closest Pair (2D):** Split by x, recurse for `δ = min(δ_L, δ_R)`, check vertical strip of width `2δ` by comparing each point to at most 7 neighbors sorted by y. Time: `Θ(n log n)`.
- **Euclid's GCD:** `gcd(a, b) = gcd(b, a mod b)`. Worst case = consecutive Fibonacci numbers. Time: `O(log(min(a, b)))`.
- **Fast Modular Exponentiation:** Square-and-multiply based on binary bits of exponent. Time: `O(log b)` multiplications.
- **Karatsuba Trick:** `X·Y = P1·10ⁿ + (P3 - P1 - P2)·10^(n/2) + P2` where `P3 = (a+b)(c+d)`. Reduces 4 mults to 3!

---

## 3. Disjoint Set Union (DSU) Summary

| Feature | Naive Sets | With Union by Rank | With Path Compression (Collapsing) | Combined (Rank + Compression) |
|---------|:----------:|:------------------:|:----------------------------------:|:----------------------------:|
| **Tree Height** | `O(n)` | `O(log n)` | Flattens tree dynamically | Effectively flat `≤ 4` |
| **Find(x)** | `O(n)` | `O(log n)` | Amortized `O(log n)` | **`O(α(n)) ≈ O(1)`** |
| **Union(x, y)** | `O(n)` | `O(log n)` | Amortized `O(log n)` | **`O(α(n)) ≈ O(1)`** |

*α(n) is Inverse Ackermann Function; α(n) ≤ 4 for all practical universe inputs.*

---

## 4. Selection Criteria: Quick Sort vs Merge Sort

| Criteria | Choose Quick Sort | Choose Merge Sort |
|----------|-------------------|-------------------|
| **Memory Constraint** | In-place sorting (`O(log n)` stack space). | Needs `O(n)` auxiliary space. Avoid if RAM is tight. |
| **Average Speed** | Faster constant factors in practice (`cache friendly`). | Slightly slower constant factor due to array copies. |
| **Worst-Case Guarantee** | Risky: `O(n²)` if data is already sorted. | Guaranteed `O(n log n)` under all circumstances. |
| **Data Structure** | Ideal for contiguous arrays. | Ideal for Linked Lists (no random access needed, `O(1)` merge). |
| **Stability** | Not stable by default. | Stable (preserves relative order of duplicate keys). |

---

## 5. Common Traps in Exams
- **Trap 1:** Forgetting `(mid - i + 1)` in Counting Inversions. *(Taking just +1 is wrong; it must invert with ALL remaining elements in left half).*
- **Trap 2:** Karatsuba middle term: it is `P3 - P1 - P2`, NOT `P3 - P1 + P2`.
- **Trap 3:** Josephus formula `J(2ᵐ + L) = 2L + 1`: here `2ᵐ` must be the LARGEST power of 2 strictly `≤ n`.
- **Trap 4:** Closest pair strip check: why at most 7 neighbors? *(Because in a δ × 2δ rectangle, at most 8 points can exist with pairwise distance ≥ δ).*
