# DAA Unit V: Network Flows Optimization — Question Bank

**Course Code**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Total Questions**: 12 (4 Short, 4 Medium, 4 Long/Numerical)  
*Note: Full worked solutions and marking schemes are provided in the separate file `DAA_Unit5_Answers.md`.*

---

## Section A: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Define a **flow network** and state the three formal properties that any valid flow function must satisfy.

### Q2. [4 Marks]
Explain the concept of a **residual network** `G_f`. Given an edge `(u, v)` with capacity `c(u, v) = 15` and current flow `f(u, v) = 9`, what are the residual capacities of the forward edge `c_f(u, v)` and the backward edge `c_f(v, u)`? Explain their physical meanings.

### Q3. [4 Marks]
Define a **cut** `(S, T)` in a flow network. State the mathematical formulas for the **net flow across the cut** `f(S, T)` and the **capacity of the cut** `c(S, T)`. What common mistake do students make regarding reverse edges?

### Q4. [4 Marks]
Why is the basic Ford-Fulkerson method considered **pseudo-polynomial**? Describe a pathological scenario where it takes an excessive number of iterations.

---

## Section B: Medium Analytical & Descriptive Questions (8 Marks Each)

### Q5. [8 Marks]
(a) State the **Max-Flow Min-Cut Theorem** and list the three conditions that are proved to be equivalent. [4M]  
(b) Explain the procedure to identify the minimum cut `(S, T)` once the maximum flow has been reached using the residual graph. [4M]

### Q6. [8 Marks]
Compare and contrast the **Ford-Fulkerson Method** and the **Edmonds-Karp Algorithm**.
Provide a structured comparison table covering:
1. Augmenting path selection criterion
2. Search algorithm used
3. Time complexity
4. Dependence on edge capacities
5. Behavior on irrational capacities

### Q7. [8 Marks]
Explain how the **Maximum Bipartite Matching (MBM)** problem is reduced to a Maximum Flow problem.
1. Draw/describe the construction of the transformed flow network `G' = (V', E')` with source `s` and sink `t`.
2. State the capacities assigned to each edge.
3. State the **Integrality Theorem** and prove why the resulting flow corresponds to a valid matching of maximum cardinality.
4. What is the overall time complexity?

### Q8. [8 Marks]
Prove the **Weak Duality Lemma** for network flows: For any feasible flow `f` and any cut `(S, T)` in a flow network `G`, prove that:
```
|f| = f(S, T) ≤ c(S, T)
```
Deduce from this that if `|f| = c(S*, T*)` for some cut `(S*, T*)`, then `f` is a maximum flow and `(S*, T*)` is a minimum cut.

---

## Section C: Long Numerical & Comprehensive Problems (10 Marks Each)

### Q9. [10 Marks]
Consider the flow network with source `s` and sink `t` having vertices `{s, A, B, C, D, t}` and edge capacities:
- `c(s, A) = 10`, `c(s, B) = 10`
- `c(A, B) = 2`, `c(A, C) = 4`, `c(A, D) = 8`
- `c(B, D) = 9`
- `c(C, t) = 10`
- `c(D, C) = 6`, `c(D, t) = 10`

1. Trace the execution of the **Ford-Fulkerson method** step-by-step. In each iteration, state:
   - The augmenting path `P` chosen.
   - The bottleneck capacity `c_f(P)`.
   - The updated flow on each edge.
2. State the final maximum flow value `|f*|`.
3. Identify the sets `S` and `T` of the **minimum cut** and calculate `c(S, T)`.
4. Verify that `|f*| = c(S, T)`.

### Q10. [10 Marks]
Consider the following directed network with source `s` and sink `t`:
- Nodes: `{s, 1, 2, 3, 4, t}`
- Capacities:
  - `(s, 1): 16`, `(s, 2): 13`
  - `(1, 2): 10`, `(1, 3): 12`
  - `(2, 1): 4`, `(2, 4): 14`
  - `(3, 2): 9`, `(3, t): 20`
  - `(4, 3): 7`, `(4, t): 4`

1. Apply the **Edmonds-Karp algorithm** (using BFS to find shortest augmenting paths by hop count) to find the maximum flow.
2. Show each augmenting path, the bottleneck capacity, and residual capacities.
3. Determine the minimum cut `(S, T)` and verify the Max-Flow Min-Cut theorem.

### Q11. [10 Marks]
A university department has 4 teaching assistants: `TA = {T1, T2, T3, T4}` and 4 courses needing a TA: `C = {C1, C2, C3, C4}`.
The eligibility/preference relationships are:
- `T1` can teach `{C1, C2}`
- `T2` can teach `{C1, C3}`
- `T3` can teach `{C2, C3, C4}`
- `T4` can teach `{C3}`

1. Formulate this as a **Maximum Bipartite Matching** problem and draw the bipartite graph. [3M]
2. Transform it into a **Maximum Flow Network** showing source `s`, sink `t`, directed edges, and all edge capacities. [3M]
3. Find the maximum matching using augmenting paths. Show all steps and the final matched pairs. [4M]

### Q12. [10 Marks]
(a) In the analysis of the **Edmonds-Karp algorithm**, prove that the shortest-path distance `δ_f(s, v)` in the residual network `G_f` increases monotonically with each flow augmentation. [6M]  
(b) Using this monotonic property, prove that each edge can become **critical** at most `|V| / 2` times, and derive the overall time complexity of `O(V · E²)`. [4M]
