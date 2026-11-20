# DAA Unit IV: Examination Questions (Greedy & Backtracking Strategy)

> **Course:** CS3212 — Design and Analysis of Algorithms  
> **Source Files Used:**
> - `DAA SYLLABUS.pdf` (Unit IV)
> - `DAA Endsem Revision (CS3212).pdf` (Part C, Q7, Q14, Q15, Q19)
> - `DAAOA_Question_Bank.docx` (Unit IV)
> - `DAA_Question_Bank_Ans_Final.docx` (Unit IV Model Answers)
>
> *Note:* In accordance with the study system rules, answers are provided in a separate file: [DAA_Unit4_Answers.md](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Questions/DAA_Unit4_Answers.md).

---

## Section A: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Define:
1. **Feasible Solution** vs **Optimal Solution** in algorithm design.
2. The **Greedy-Choice Property** and the **Optimal Substructure** property.  
*(Source: DAAOA_Question_Bank.docx Unit IV Q1, Q2)*

### Q2. [4 Marks]
Differentiate clearly between **Prim’s Algorithm** and **Kruskal’s Algorithm** for finding a Minimum Spanning Tree (MST). State which algorithm is preferable for dense graphs versus sparse graphs and explain why.  
*(Source: DAAOA_Question_Bank.docx Unit IV Q10 & Endsem Revision Part C Q19)*

### Q3. [4 Marks]
State the **Cut Property** of Minimum Spanning Trees. Using the cut property, explain why the lightest edge crossing any cut `(S, V - S)` must belong to an MST.  
*(Source: DAA SYLLABUS.pdf Unit IV & CLRS Chapter 23)*

### Q4. [4 Marks]
Explain the greedy rule for the **Conflict-Free Activity Scheduling** problem. Prove why selecting the activity with the earliest finish time is an optimal choice.  
*(Source: DAA SYLLABUS.pdf Unit IV & Endsem Revision Part A Unit IV)*

### Q5. [4 Marks]
In the **N-Queens Problem**, state the mathematical condition used by the backtracking algorithm to determine if two queens placed at `(i, X[i])` and `(k, X[k])` attack each other along a diagonal. Give an example on a 4×4 board.  
*(Source: DAA Endsem Revision (CS3212).pdf Part C Q15)*

### Q6. [4 Marks]
Differentiate between **Backtracking** and **Branch-and-Bound** in terms of state-space tree traversal, data structures used, and the types of computational problems they are designed to solve.  
*(Source: DAAOA_Question_Bank.docx Unit V Q16(b))*

---

## Section B: Long / Numerical Questions (8 to 10 Marks Each)

### Q7. [10 Marks]
a) Construct a **Huffman Tree** for the following set of character frequencies:  
```
Character:    a     b     c     d     e     f
Frequency:   45    13    12    16     9     5
```
Show the step-by-step min-heap merging process, draw the final Huffman binary tree, and assign binary codewords (`0` for left, `1` for right). **[6 Marks]**  
b) Calculate the **average codeword length** in bits per character. How many total bits are saved compared to a fixed-length 3-bit binary encoding? **[4 Marks]**  
*(Source: DAA Endsem Revision (CS3212).pdf Part C Q14)*

### Q8. [10 Marks]
a) Explain the **Backtracking Strategy** for the **4-Queens Problem**. Draw the complete state-space tree showing all explored branches, dead ends (pruned subtrees), and backtracks. State the first valid solution vector found. **[6 Marks]**  
b) Write the backtracking algorithm for the general `N-Queens` problem and state its worst-case time and space complexity. **[4 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit II Q11(a), Q14(b) & Endsem Revision Part C Q15)*

### Q9. [10 Marks]
a) Given the set of weights `W = (11, 13, 24, 7)` and target sum `M = 31`:  
Solve the **Sum of Subsets Problem** using the backtracking algorithm.  
1. Sort the weights in ascending order.  
2. Draw the portion of the state-space tree using the fixed-tuple sized approach (`x_i ∈ {0, 1}`).  
3. Clearly label where subtrees are pruned using the bounding functions.  
4. Identify all valid solution subsets that sum to `31`. **[7 Marks]**  
b) State the two bounding functions (left pruning and right pruning) used in the Sum of Subsets problem. **[3 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit II Q12(a))*

### Q10. [8 Marks]
Find an optimal job schedule and maximum profit for the following instance of `n = 5` jobs with deadlines and profits:  
```
Job:        J1    J2    J3    J4    J5
Deadline:    2     2     1     3     3
Profit:     20    15    10     5     1
```
Each job requires 1 unit of time. Show the step-by-step slot allocation process, list the final scheduled job sequence, and compute the total profit earned. **[8 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit IV Q13(a) & DAA_Question_Bank_Ans_Final.docx)*

### Q11. [8 Marks]
a) Explain **Dijkstra’s Algorithm** for Single Source Shortest Path. Trace its steps and state why it requires all edge weights to be non-negative. **[5 Marks]**  
b) Explain the **Graph m-Colouring Problem**. Define the **Chromatic Number** of a graph and illustrate the backtracking state-space tree for coloring a 4-vertex graph using `m = 3` colors. **[3 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit II Q13(a, b) & Unit IV Q16(a))*
