# DAA Unit IV: Visual Exam Diagrams

> **Course:** CS3212 — Design and Analysis of Algorithms  
> **Topic:** Unit IV (Greedy & Backtracking Strategy)  
> **Note:** High-resolution PNG image files are saved directly in this folder. Below are the visual sketches, exam redraw tips, and direct links to the generated PNG images.

---

## Diagram 1: Huffman Coding Tree Construction
- **High-Resolution PNG:** [DAA_Unit4_Diagram1_HuffmanTree.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit4_Diagram1_HuffmanTree.png)

### Visual Sketch & Exam Redraw Guide:
```text
                     [ Root: 100 ]
                     /           \
             (0)    /             \  (1)
                [ a: 45 ]       [ (cbefd): 55 ]
                                  /         \
                          (0)    /           \  (1)
                             [ (cb): 25 ]   [ (efd): 30 ]
                                /     \         /     \
                        (0)    /   (1) \ (0)   /   (1) \\
                           [c:12]   [b:13] [ (ef):14 ]  [d:16]
                                             /     \
                                     (0)    /   (1) \\
                                         [f:5]     [e:9]

Codewords:
  a: 0      (1 bit)    |  d: 111     (3 bits)
  c: 100    (3 bits)   |  f: 1100    (4 bits)
  b: 101    (3 bits)   |  e: 1101    (4 bits)

Average Code Length = (45*1 + 13*3 + 12*3 + 16*3 + 9*4 + 5*4) / 100 = 2.24 bits/char
Bits saved vs 3-bit fixed encoding: 300 - 224 = 76 bits (25.3% savings!)
```

---

## Diagram 2: 4-Queens Backtracking State-Space Tree
- **High-Resolution PNG:** [DAA_Unit4_Diagram2_FourQueensTree.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit4_Diagram2_FourQueensTree.png)

### Visual Sketch & Exam Redraw Guide:
```text
                       [ Root: Empty Board ]
                        /                 \
            X[1] = 1   /                   \  X[1] = 2
                      v                     v
                [ Q1 at (1,1) ]       [ Q1 at (1,2) ]
                 /           \               |
        X[2]=3  /     X[2]=4  \       X[2]=4 |
               v               v             v
         [ Q2 at (2,3) ] [ Q2 at (2,4) ] [ Q2 at (2,4) ]
               |               |             |
           (All cols       X[3]=2 |       X[3]=1 |
           attacked!)          v             v
             PRUNE!      [ Q3 at (3,2) ] [ Q3 at (3,1) ]
                               |             |
                           (All cols      X[4]=3 |
                           attacked!)        v
                             PRUNE!     [ Q4 at (4,3) ]  <-- ALL QUEENS SAFE!
                                               |
                                     VALID SOLUTION: [2, 4, 1, 3]!
```

---

## Diagram 3: Sum of Subsets State-Space Tree (W = {7, 11, 13, 24}, M = 31)
- **High-Resolution PNG:** [DAA_Unit4_Diagram3_SubsetSumTree.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit4_Diagram3_SubsetSumTree.png)

### Visual Sketch & Exam Redraw Guide:
```text
                             [ s=0, r=55 ]
                              /         \
                      x1=1   /           \  x1=0
                            v             v
                     [ s=7, r=48 ]    [ s=0, r=48 ]
                      /         \           \
              x2=1   /     x2=0  \           \ x2=1 (s=11)
                    v             v           ...
             [ s=18, r=37 ]   [ s=7, r=37 ]
              /         \         |
      x3=1   /     x3=0  \        | x3=0 (s=7, r=24)
            v             v       v
      [ s=31 ]!       [ s=18 ]  [ s=7 ]
    TARGET REACHED!      |        | x4=1
   Sol 1: {7, 11, 13}    | x4=1   v
                         v     [ s=31 ]! TARGET REACHED!
                      [ s=42 ]  Sol 2: {7, 24}
                     PRUNE >31!

Bounding Functions to Write:
1. Right Prune: If (current_sum + w_(i+1) > M) -> PRUNE!
2. Left Prune:  If (current_sum + remaining_sum < M) -> PRUNE!
```

---

## Diagram 4: Minimum Spanning Tree: Prim vs Kruskal & Cut Property
- **High-Resolution PNG:** [DAA_Unit4_Diagram4_PrimVsKruskal.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit4_Diagram4_PrimVsKruskal.png)

### Visual Sketch:
```text
Cut Property:
  For any cut (S, V - S), the minimum weight edge crossing the cut belongs to an MST.

  [ Set S (In Tree) ]   ========= Lightest Crossing Edge (u, v) =========>  [ Set V - S (Outside) ]

1. Prim's Algorithm (Vertex-Centric):
   - Grows a single connected tree S from start vertex.
   - At each step, picks the cheapest edge crossing cut (S, V - S) via Min-Heap.
   - Time: O(E log V) with heap / O(V^2) with matrix. Best for DENSE graphs.

2. Kruskal's Algorithm (Edge-Centric):
   - Sorts all edges globally: e1 <= e2 <= ... <= e_m.
   - Inserts edges one by one; uses Disjoint Set Union (DSU) to prevent cycles.
   - Time: O(E log E) = O(E log V). Best for SPARSE graphs.
```

---

## Diagram 5: Graph m-Coloring State-Space Tree & Neighbor Conflict Pruning
- **High-Resolution PNG:** [DAA_Unit4_Diagram5_GraphColoringTree.png](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Diagrams/DAA_Unit4_Diagram5_GraphColoringTree.png)

### Visual Sketch:
```text
                   [ Root: No Vertices Colored ]
                    /            |            \
          Color 1  /     Color 2 |     Color 3 \
                  v              v              v
           [ V1 = Red ]   [ V1 = Green ]  [ V1 = Blue ]
            /    |    \
     C=1   / C=2 | C=3 \
          v      v      v
      [ V2=R ] [V2=G]  [V2=B]
       PRUNE!
     (Adjacent
      to V1!)

Chromatic Number chi(G): Smallest m such that G is m-colorable.
Complexity: Worst case state-space tree has m^n nodes -> O(m^n).
```
