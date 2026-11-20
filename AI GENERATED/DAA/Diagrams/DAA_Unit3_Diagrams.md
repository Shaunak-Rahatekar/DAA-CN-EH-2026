# DAA Unit III: Visual Exam Diagrams

> **Course:** CS3212 — Design and Analysis of Algorithms  
> **Topic:** Unit III (Dynamic Programming)  
> **Note:** High-resolution PNG image files are saved directly in this folder. Below are the visual sketches, exam redraw tips, and direct links to the generated PNG images.

---

## Diagram 1: Longest Common Subsequence (LCS) Tabulation & Traceback
- **High-Resolution PNG:** [DAA_Unit3_Diagram1_LCS_Table.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit3_Diagram1_LCS_Table.png)

### Visual Sketch & Exam Redraw Guide:
```text
Table for X = "ABCBDAB" (m=7) and Y = "BDCABA" (n=6):

        -    B    D    C    A    B    A
   - [  0    0    0    0    0    0    0  ]
   A [  0    0    0    0    1    1    1  ]
   B [  0   [1]   1    1    1    2    2  ]  <-- 'B' matched! (diag to 1,0)
   C [  0    1    1   [2]   2    2    2  ]  <-- 'C' matched! (diag to 2,2)
   B [  0    1    1    2    2   [3]   3  ]  <-- 'B' matched! (diag to 3,4)
   D [  0    1    2    2    2    3    3  ]
   A [  0    1    2    2    3    3   [4] ]  <-- 'A' matched! (diag to 5,5)
   B [  0    1    2    2    3    4    4  ]  <-- Start here at (7,6), move UP to (6,6)

========================================================================
EXAM TRACEBACK RULES:
1. If X[i] == Y[j]: Character is part of LCS! Take character, move DIAGONALLY to (i-1, j-1).
2. If X[i] != Y[j]: Move to the larger adjacent neighbor (UP or LEFT).
3. Traceback Path: (7,6) -> (6,6) -> (5,5) -> (4,5) -> (3,4) -> (3,3) -> (2,2) -> (2,1) -> (1,0)
4. Extracted Sequence: "BCBA" (Length = 4).
========================================================================
```

---

## Diagram 2: 0/1 Knapsack Tabulation (Capacity W = 7) & Item Traceback
- **High-Resolution PNG:** [DAA_Unit3_Diagram2_Knapsack_Table.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit3_Diagram2_Knapsack_Table.png)

### Visual Sketch & Exam Redraw Guide:
```text
Items: 1(w=1,v=1), 2(w=3,v=4), 3(w=4,v=5), 4(w=5,v=7)

   w:      0   1   2   3   4   5   6   7
i=0 (base) 0   0   0   0   0   0   0   0
i=1 (w=1)  0   1   1   1   1   1   1   1
i=2 (w=3)  0   1   1  [4]  5   5   5   5   <-- TAKEN! (w = 3 - 3 = 0)
i=3 (w=4)  0   1   1   4   5   6   6  [9]  <-- TAKEN! (w = 7 - 4 = 3)
i=4 (w=5)  0   1   1   4   5   7   8   9   <-- NOT taken (value unchanged from i=3)

========================================================================
EXAM TRACEBACK RULES:
1. Start at cell K[4, 7] = 9.
2. Since K[4, 7] == K[3, 7] (both are 9), Item 4 was NOT included. Move to row 3.
3. Since K[3, 7] (9) != K[2, 7] (5), Item 3 WAS included!
   Subtract weight: w = 7 - w3 = 7 - 4 = 3. Move to cell K[2, 3].
4. Since K[2, 3] (4) != K[1, 3] (1), Item 2 WAS included!
   Subtract weight: w = 3 - w2 = 3 - 3 = 0.
5. Capacity reached 0. Optimal set: {Item 2, Item 3}. Total Value = 9, Weight = 7.
========================================================================
```

---

## Diagram 3: Matrix Chain Multiplication (Diagonal Fill & Parenthesization)
- **High-Resolution PNG:** [DAA_Unit3_Diagram3_MatrixChainOrder.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit3_Diagram3_MatrixChainOrder.png)

### Visual Sketch & Exam Redraw Guide:
```text
Matrices: A1(3x2), A2(2x4), A3(4x2), A4(2x5)   p = [3, 2, 4, 2, 5]

m Table:           1       2       3       4
           1  [    0      24      28      58  ] <-- L=4: Final answer m[1,4] = 58
           2  [    -       0      16      36  ] <-- L=3: m[1,3], m[2,4]
           3  [    -       -       0      40  ] <-- L=2: m[1,2], m[2,3], m[3,4]
           4  [    -       -       -       0  ] <-- L=1: Base cases (diagonal = 0)

Split Table s[i, j]:
           s[1,2]=1,  s[2,3]=2,  s[3,4]=3
           s[1,3]=1,  s[2,4]=3
           s[1,4]=3  ==> Split at 3: (A1 A2 A3) · (A4)
                         Then s[1,3]=1: (A1) · (A2 A3)

Resulting Binary Tree Parenthesization:
                     (A1 · A2 · A3 · A4)
                           /     \
                (A1 · A2 · A3)    (A4)
                    /     \
                 (A1)   (A2 · A3)
                         /     \
                       (A2)   (A3)
Final Parenthesization: ((A1 · (A2 · A3)) · A4) with 58 Multiplications
```

---

## Diagram 4: Floyd-Warshall Waypoint Relaxation Principle
- **High-Resolution PNG:** [DAA_Unit3_Diagram4_FloydWarshallStep.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit3_Diagram4_FloydWarshallStep.png)

### Visual Sketch & Exam Redraw Guide:
```text
Direct Edge:
  [ Vertex i ] ---------------- D^(k-1)[i, j] ----------------> [ Vertex j ]

Alternative via Waypoint k:
  [ Vertex i ] ------- D^(k-1)[i, k] -------> [ Vertex k ]
                                                     |
                                                     |  D^(k-1)[k, j]
                                                     v
                                              [ Vertex j ]

Recurrence:
  D^(k)[i, j] = min( D^(k-1)[i, j],   D^(k-1)[i, k] + D^(k-1)[k, j] )

Negative Cycle Detection Rule:
  If after phase k = n, any diagonal entry D[i, i] < 0, a Negative Weight Cycle exists!
```

---

## Diagram 5: Optimal Binary Search Tree (OBST) Subtree Structure
- **High-Resolution PNG:** [DAA_Unit3_Diagram5_OBST_Structure.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit3_Diagram5_OBST_Structure.png)

### Visual Sketch & Exam Redraw Guide:
```text
                    [ Root Key Kr ]  <-- Candidate r (i <= r <= j)
                       /         \
                      /           \
           Left Subtree            Right Subtree
           Keys: Ki .. K(r-1)      Keys: K(r+1) .. Kj
           Cost: e[i, r-1]         Cost: e[r+1, j]

========================================================================
EXAM INTUITION:
When Kr is selected as root, all nodes in Left Subtree and Right Subtree
are pushed one level deeper in the tree (their depth increases by 1).
Adding 1 to the depth of each node adds the sum of all their probabilities:
w(i, j) = sum of all p_k and q_k in the range.

Optimal Cost Equation:
  e[i, j] = min (i <= r <= j) { e[i, r - 1] + e[r + 1, j] + w(i, j) }
  Time: O(n³)  |  Space: O(n²)
========================================================================
```
