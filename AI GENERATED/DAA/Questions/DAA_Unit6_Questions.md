# DAA Unit VI: Introduction to Theory of NP-Completeness — Question Bank

**Course Code**: CS3212 | **Subject**: Design and Analysis of Algorithms  
**Total Questions**: 12 (4 Short, 4 Medium, 4 Long/Analytical)  
*Note: Full worked solutions and marking schemes are provided in the separate file `DAA_Unit6_Answers.md`.*

---

## Section A: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Define the complexity classes **P**, **NP**, **NP-Hard**, and **NP-Complete**. Draw a clear Venn diagram showing their relationship under the widely accepted assumption that `P ≠ NP`.

### Q2. [4 Marks]
Explain the difference between a **deterministic polynomial-time algorithm** and a **non-deterministic polynomial-time algorithm**. Why does "NP" not mean "Non-Polynomial"?

### Q3. [4 Marks]
State the **Cook-Levin Theorem**. Why is this theorem considered the foundational cornerstone of NP-Completeness theory?

### Q4. [4 Marks]
Differentiate between **Las Vegas** and **Monte Carlo** randomized algorithms in terms of correctness, running time, and give one classic example of each.

---

## Section B: Medium Analytical & Descriptive Questions (8 Marks Each)

### Q5. [8 Marks]
(a) Define a **polynomial-time reduction** `A ≤_p B`. Explain what it means when we say that "hardness transfers from left to right, while solvability transfers from right to left". [4M]  
(b) Outline the standard **4-step template** used to prove that a newly proposed decision problem `L` is NP-Complete. [4M]

### Q6. [8 Marks]
Prove that the **Independent Set** problem and the **Vertex Cover** problem are polynomially equivalent:
1. Define both problems formally as decision problems. [2M]
2. State and prove the theorem: A subset `S ⊆ V` is an independent set in graph `G = (V, E)` if and only if `V - S` is a vertex cover in `G`. [4M]
3. Show how an algorithm for one problem can be used to solve the other in polynomial time. [2M]

### Q7. [8 Marks]
Explain how the **3-SAT** problem is reduced to the **Clique** problem (`3-SAT ≤_p Clique`):
1. Give the formal construction of the graph `G` and target clique size `k` from a 3-CNF formula `φ` with `m` clauses. [4M]
2. Prove the forward direction: If `φ` is satisfiable, then `G` contains a clique of size `m`. [2M]
3. Prove the reverse direction: If `G` contains a clique of size `m`, then `φ` is satisfiable. [2M]

### Q8. [8 Marks]
(a) Distinguish between **Linear Programming (LP)** and **Integer Linear Programming (ILP)** in terms of variable constraints and computational complexity. [3M]  
(b) Formulate the **Vertex Cover problem** as a 0-1 Integer Linear Program, defining the decision variables, objective function, and constraints clearly. [3M]  
(c) Formulate the **0/1 Knapsack problem** as a 0-1 Integer Linear Program. [2M]

---

## Section C: Long Comprehensive & Proof-Based Problems (10 Marks Each)

### Q9. [10 Marks]
(a) What is an **approximation algorithm**? Define the **approximation ratio ρ(n)** for both minimization and maximization problems. [3M]  
(b) Describe the polynomial-time **2-approximation algorithm for the Vertex Cover problem** based on maximal matching. [3M]  
(c) Give the complete mathematical proof showing that the algorithm achieves an approximation ratio of `ρ = 2`. [4M]

### Q10. [10 Marks]
(a) Define the **Metric Traveling Salesperson Problem (Metric TSP)** and state the **Triangle Inequality**. [2M]  
(b) Present the **2-approximation algorithm for Metric TSP** using the Minimum Spanning Tree (MST) doubling and shortcutting approach. [4M]  
(c) Prove rigorously that the cost of the tour returned by the algorithm is strictly less than twice the cost of the optimal TSP tour (`Cost(H) ≤ 2 · Cost(H*)`). [4M]

### Q11. [10 Marks]
(a) State the **Halting Problem** formally. [2M]  
(b) Prove that the Halting Problem is **undecidable** using Alan Turing's diagonalization argument by contradiction. [5M]  
(c) Explain why the Halting Problem is **NP-Hard** yet does **not** belong to the class NP. [3M]

### Q12. [10 Marks]
Consider **Karger's randomized algorithm for the Global Minimum Cut** in an undirected multigraph:
(a) Describe the step-by-step edge contraction procedure of Karger's algorithm. [3M]  
(b) Prove that the probability that a single run of Karger's algorithm finds the true minimum cut is at least `2 / (n · (n - 1))`. [4M]  
(c) Show how repeating the algorithm `k = (n² / 2) · ln(n)` times reduces the probability of failure to at most `1 / n`. [3M]
