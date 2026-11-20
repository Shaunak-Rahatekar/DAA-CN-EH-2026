# End-Semester Examination: Design and Analysis of Algorithms (CS3212)

**Course Code**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Duration**: 2 Hours (11:00 AM – 1:00 PM) | **Maximum Marks**: 60  
*Instructions*:
1. Answer **ALL** questions in Section A (Compulsory).
2. Answer any **TWO** questions from Section B.
3. Answer any **TWO** questions from Section C.
4. Draw neat, labeled diagrams and show step-by-step trace tables where required.
5. Mathematical notation should be clear. Universal Unicode notation is used throughout.

---

## SECTION A: Short Answer Conceptual Questions (Compulsory)
**Answer all 5 questions. Each question carries 4 marks.** (5 × 4 = 20 Marks)

### Q1. [4 Marks]
(a) State the formal mathematical definition of **Big-Θ (Theta)** notation with bounding constants `c1, c2, n0`. [2M]  
(b) Using the formal definition, prove that `f(n) = 4n² + 3n = Θ(n²)`. [2M]

### Q2. [4 Marks]
Solve the following recurrence relations using the **Master Theorem** or state why the Master Theorem does not apply:
(a) `T(n) = 4T(n/2) + n²` [2M]  
(b) `T(n) = 2T(n/2) + n log n` [2M]

### Q3. [4 Marks]
State the **Optimal Substructure** property and the **Overlapping Subproblems** property. Give one example of a problem that has optimal substructure but does NOT have overlapping subproblems. [4M]

### Q4. [4 Marks]
In a flow network, define a **cut (S, T)** and state the mathematical formula for its **capacity c(S, T)**. Why are reverse edges from `T` to `S` excluded when computing the cut capacity? [4M]

### Q5. [4 Marks]
Define the complexity class **NP-Complete**. If an algorithm of time complexity `O(n⁴)` is discovered for the 3-SAT problem tomorrow, what would be the exact consequence for the relationship between class P and class NP? Explain why. [4M]

---

## SECTION B: Medium Analytical & Derivation Questions
**Answer any TWO questions from Q6, Q7, and Q8. Each carries 8 marks.** (2 × 8 = 16 Marks)

### Q6. [8 Marks]
(a) Explain the difference between **Divide and Conquer**, **Dynamic Programming**, and the **Greedy Strategy** in terms of subproblem independence, choice timing (top-down vs bottom-up), and memoization. Provide a structured comparison table. [5M]  
(b) State the **Adversary Decision Tree Lower Bound Theorem** for comparison-based sorting, and prove why no comparison sort can run in faster than `Ω(n log n)` in the worst case. [3M]

### Q7. [8 Marks]
(a) Describe the **4-Queens Problem**. Formulate the state-space representation and explain how the backtracking bounding function prunes invalid states. [4M]  
(b) Contrast **Backtracking** with **Branch and Bound** in terms of state-space tree traversal order, node expansion strategy, and problem types solved. [4M]

### Q8. [8 Marks]
(a) Describe how the **Maximum Bipartite Matching (MBM)** problem is reduced to a Maximum Flow problem. State the capacities assigned to each directed edge and cite the **Integrality Theorem** that guarantees a discrete 0-1 matching. [5M]  
(b) Differentiate between **Las Vegas** and **Monte Carlo** randomized algorithms in terms of output correctness and running time, giving one example of each. [3M]

---

## SECTION C: Long Numerical & Comprehensive Problems
**Answer any TWO questions from Q9, Q10, and Q11. Each carries 12 marks.** (2 × 12 = 24 Marks)

### Q9. [12 Marks] (Dynamic Programming)
(a) Given two sequences:
    `X = <A, B, C, B, D, A, B>`
    `Y = <B, D, C, A, B, A>`
    1. Construct the complete `(m+1) × (n+1)` dynamic programming table `c[i, j]` for the **Longest Common Subsequence (LCS)**, showing cell values and directional arrows (`↑`, `←`, `↖`). [6M]
    2. Extract the longest common subsequence and state its length. [2M]
(b) Given a 0/1 Knapsack problem with knapsack capacity `W = 7` and 4 items:
    - Item 1: Weight `w1 = 2`, Value `v1 = 3`
    - Item 2: Weight `w2 = 3`, Value `v2 = 4`
    - Item 3: Weight `w3 = 4`, Value `v3 = 5`
    - Item 4: Weight `w4 = 5`, Value `v4 = 8`
    Construct the dynamic programming table `K[i, w]` for `i = 0..4` and `w = 0..7`. Determine the maximum value and identify the specific items included in the optimal knapsack. [4M]

### Q10. [12 Marks] (Network Flows & Min-Cut)
Consider the flow network with source `s` and sink `t` having the following directed edge capacities:
- `c(s, 1) = 16`, `c(s, 2) = 13`
- `c(1, 2) = 10`, `c(1, 3) = 12`
- `c(2, 1) = 4`, `c(2, 4) = 14`
- `c(3, 2) = 9`, `c(3, t) = 20`
- `c(4, 3) = 7`, `c(4, t) = 4`

1. Trace the execution of the **Edmonds-Karp algorithm** (using BFS to find shortest augmenting paths by hop count) to find the maximum flow. For each augmenting path, state:
   - The path sequence from `s` to `t`.
   - The bottleneck capacity `c_f(P)`.
   - The updated flow on each edge. [7M]
2. State the final maximum flow value `|f*|`. [1M]
3. Construct the final residual graph `G_f`. Determine the reachable set `S` and unreachable set `T` from source `s`. [2M]
4. Identify the edges crossing the cut from `S` to `T`, compute the cut capacity `c(S, T)`, and verify the **Max-Flow Min-Cut Theorem**. [2M]

### Q11. [12 Marks] (NP-Completeness & Approximations)
(a) Prove that the **Independent Set** problem is NP-Complete by reducing **3-SAT** to **Independent Set** (or via Clique). Give the reduction construction, state the target parameter, and prove correctness in both directions. [6M]  
(b) For the **Vertex Cover problem**:
    1. Describe the polynomial-time **2-approximation algorithm** based on maximal matching. [2M]
    2. Prove mathematically that the algorithm achieves an approximation ratio of `ρ ≤ 2`. [4M]
