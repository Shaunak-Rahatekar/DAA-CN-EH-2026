# DAA Unit II: Visual Exam Diagrams

> **Course:** CS3212 — Design and Analysis of Algorithms  
> **Topic:** Unit II (Recursion, Divide and Conquer & Combinatorial Search)  
> **Note:** High-resolution PNG image files are saved directly in this folder. Below are the visual sketches, exam redraw tips, and direct links to the generated PNG images.

---

## Diagram 1: Closest Pair of Points in 2D Plane (The 2δ Strip Search)
- **High-Resolution PNG:** [DAA_Unit2_Diagram1_ClosestPairStrip.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit2_Diagram1_ClosestPairStrip.png)

### Visual Sketch & Exam Redraw Guide:
```text
           Y
           ^
           |     Left Region (δ_L)      |    Right Region (δ_R)
           |             *              |             *
           |                  *         |         *
           |       *                    |                   *
           |             +--------------+--------------+
           |             |              |  p *         |
           |             |   *          |              |  <-- δ × 2δ Box
           |             |              |        *     |      (At most 8 points fit;
           |             +--------------+--------------+       check at most 7 neighbors!)
           |                 *          |           *
           |                            |      *
           +-------------+--------------+--------------+--------> X
           0           mid - δ       x = mid        mid + δ
                         |<-------- 2δ Strip --------->|
```

### Key Exam Formula to Write Beside Diagram:
1. `δ = min(δ_Left, δ_Right)`.
2. Any cross-boundary pair must lie inside a vertical strip of width `2δ` around `x = mid`.
3. In a rectangle of dimension `2δ × δ`, by geometric packing, at most 8 points can exist with pairwise distance `≥ δ`.
4. Excluding point `p` itself, `p` needs to be checked against **at most 7 neighbors** sorted by y-coordinate.

---

## Diagram 2: Karatsuba Multiplication Breakdown (3 Sub-Products)
- **High-Resolution PNG:** [DAA_Unit2_Diagram2_KaratsubaMultiplication.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit2_Diagram2_KaratsubaMultiplication.png)

### Visual Sketch:
```text
               Input: X = 1234, Y = 5678 (n = 4 digits)
                          /                  \
              X = 12 · 10² + 34          Y = 56 · 10² + 78
              (a = 12, b = 34)           (c = 56, d = 78)
                     |                          |
                     +------------+-------------+
                                  |
              +-------------------+-------------------+
              |                   |                   |
        P1 = a · c          P2 = b · d          P3 = (a + b)(c + d)
       12 × 56 = 672       34 × 78 = 2652       46 × 134 = 6164
              |                   |                   |
              +-------------------+-------------------+
                                  |
               Middle Term = P3 - P1 - P2 = 2840
                                  |
        Combine: P1 · 10⁴ + Middle · 10² + P2 = 7,006,652
```

---

## Diagram 3: Median-of-Medians (Deterministic O(n) Selection)
- **High-Resolution PNG:** [DAA_Unit2_Diagram3_MedianOfMedians.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit2_Diagram3_MedianOfMedians.png)

### Visual Sketch:
```text
         Step 1: Divide n elements into blocks of 5 elements each
                 [o o x o o]   [o o x o o]   [o o x o o] ...
                      |             |             |
         Step 2: Find median of each 5-element block
                      m1            m2            m3     ... (n/5 medians)
                      \             |             /
         Step 3: Recursively find Median of Medians  ==>  Pivot x
                                    |
         Step 4: At least 3 elements per block in half the blocks are ≥ x
                 Guarantees at least 3 · (n/10) = 3n/10 elements are ≥ x
                 AND at least 3n/10 elements are ≤ x!
                                    |
         Step 5: Worst-case subproblem size ≤ 7n/10
                 Recurrence: T(n) ≤ T(n/5) + T(7n/10) + O(n)
                 ==> T(n) = Θ(n) strictly guaranteed!
```

---

## Diagram 4: Disjoint Set Union (Rank & Path Compression)
- **High-Resolution PNG:** [DAA_Unit2_Diagram4_DisjointSetUnion.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit2_Diagram4_DisjointSetUnion.png)

### Visual Sketch:
```text
(A) UNION BY RANK: Always attach smaller rank tree under larger root
       Tree 1 (Rank 2)        Tree 2 (Rank 1)          Result Tree (Rank 2)
           [ Root A ]             [ Root B ]                 [ Root A ]
            /      \                  |                       /   |   \
          [C]      [D]               [E]                    [C]  [D]  [ Root B ]
                                                                          |
                                                                         [E]

(B) PATH COMPRESSION: Reparent all visited nodes directly to the root
       Before Find(E):                           After Find(E):
          [ Root R ]                                [ Root R ]
              ^                                    /    |     \
              |                                  [A]   [B]    [E]
             [A]                                (All flattened directly to Root!)
              ^                                 Subsequent queries = O(1)!
              |
             [B]
              ^
              |
             [E]
```

---

## Diagram 5: Josephus Elimination & Binary Left-Shift Trick
- **High-Resolution PNG:** [DAA_Unit2_Diagram5_JosephusCycle.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit2_Diagram5_JosephusCycle.png)

### Visual Sketch:
```text
                       Circle of n = 41 People:
                              (1) ---> (2 eliminated)
                            /     \
                         (41)     (3) ---> (4 eliminated)
                          |        |
                         (39)     (5) ...
                          \       /
                           ( survivor = 19 )

      =========================================================
      EXAM SHORTCUT:
      n = 41 = 2⁵ + 9  ==>  m = 5, L = 9
      Formula: J(n) = 2L + 1 = 2(9) + 1 = 19

      Binary Bit-Rotation Trick:
      41 in binary = 1  0  1  0  0  1
                     |
                     +-------------------+
                                         v
      Rotate leftmost 1 to end ==> 0  1  0  0  1  1
      010011₂ in decimal = 16 + 2 + 1 = 19!
      =========================================================
```
