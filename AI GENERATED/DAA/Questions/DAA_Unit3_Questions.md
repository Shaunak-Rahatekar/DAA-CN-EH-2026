# DAA Unit III: Examination Questions (Dynamic Programming)

> **Course:** CS3212 — Design and Analysis of Algorithms  
> **Source Files Used:**
> - `DAA SYLLABUS.pdf` (Unit III)
> - `DAA Endsem Revision (CS3212).pdf` (Part C, Q6, Q7, Q12, Q13, Q17)
> - `DAAOA_Question_Bank.docx` (Unit III)
> - `DAA_Question_Bank_Ans_Final.docx` (Unit III Model Answers)
>
> *Note:* In accordance with the study system rules, answers are provided in a separate file: [DAA_Unit3_Answers.md](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Questions/DAA_Unit3_Answers.md).

---

## Section A: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Explain the **Principle of Optimality** and define the two essential characteristics a computational problem must possess to be solved using Dynamic Programming. Give one concrete example of each.  
*(Source: DAAOA_Question_Bank.docx Unit III Q8, Q9, Q11(a) & Revision Guide Part C Q6)*

### Q2. [4 Marks]
Differentiate clearly between:
1. Top-Down Dynamic Programming (Memoization) and Bottom-Up Dynamic Programming (Tabulation).
2. Dynamic Programming and the Greedy Method.  
*(Source: DAAOA_Question_Bank.docx Unit III Q10, Q15(a))*

### Q3. [4 Marks]
Explain why the **Greedy approach works for Fractional Knapsack** but fails for the **0/1 Knapsack problem**. Provide a simple counterexample demonstrating greedy failure.  
*(Source: DAA Endsem Revision (CS3212).pdf Part C Q7)*

### Q4. [4 Marks]
Why is the time complexity of the 0/1 Knapsack problem `O(n · W)` referred to as **pseudo-polynomial** rather than purely polynomial? Explain the mathematical distinction.  
*(Source: DAA SYLLABUS.pdf Unit III & CLRS Chapter 34)*

### Q5. [4 Marks]
Write the recurrence relation for the **Longest Common Subsequence (LCS)** problem. For sequences `X = "ABCBDAB"` and `Y = "BDCABA"`, state the length of the LCS and two valid LCS subsequences.  
*(Source: DAA Endsem Revision (CS3212).pdf Part C Q12)*

### Q6. [4 Marks]
How does the **Floyd-Warshall algorithm** detect the presence of a negative-weight cycle in a directed graph? State its recurrence relation and time complexity.  
*(Source: DAA Endsem Revision (CS3212).pdf Part C Q17)*

---

## Section B: Long / Numerical Questions (8 to 10 Marks Each)

### Q7. [10 Marks]
Given a sequence of four matrices with dimensions:
```
A1: 3 × 2,   A2: 2 × 4,   A3: 4 × 2,   A4: 2 × 5
```
a) Set up the dynamic programming recurrence for **Matrix Chain Multiplication**. **[3 Marks]**  
b) Construct and fill the complete cost table `m[1..4, 1..4]` and split table `s[1..4, 1..4]`. **[5 Marks]**  
c) Determine the minimum number of scalar multiplications required and the optimal parenthesization of the product `A1 · A2 · A3 · A4`. **[2 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit III Q17 & DAA_Question_Bank_Ans_Final.docx)*

### Q8. [10 Marks]
a) Solve the following instance of the **0/1 Knapsack Problem** using Dynamic Programming for a knapsack of capacity `W = 7`:  
- Item 1: `w1 = 1, v1 = 1`  
- Item 2: `w2 = 3, v2 = 4`  
- Item 3: `w3 = 4, v3 = 5`  
- Item 4: `w4 = 5, v4 = 7`  
Construct the complete dynamic programming table `K[0..4, 0..7]`. **[6 Marks]**  
b) Perform the traceback procedure on the table to determine the exact subset of items that produce the optimal value. State the total weight and maximum value achieved. **[4 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit III Q18 & DAA_Question_Bank_Ans_Final.docx)*

### Q9. [10 Marks]
Find the **Longest Common Subsequence (LCS)** of the sequences:
```
X = "ABCBDAB"   and   Y = "BDCABA"
```
a) Construct the complete `(7 + 1) × (6 + 1)` tabulation table `L[i, j]`. **[6 Marks]**  
b) Show the traceback path from cell `L[7, 6]` to cell `L[0, 0]` and extract the final sequence. **[4 Marks]**  
*(Source: DAA Endsem Revision (CS3212).pdf Part C Q12)*

### Q10. [8 Marks]
a) Explain the **Held-Karp Dynamic Programming algorithm** for the Travelling Salesperson Problem (TSP). State its recurrence relation, time complexity, and auxiliary space complexity. Why is this considered superior to brute-force permutation search? **[5 Marks]**  
b) Describe how shortest paths from a single source can be computed in a **Directed Acyclic Graph (DAG)** using topological sorting in linear time `Θ(V + E)`. **[3 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit III Q11(a) & DAA Endsem Revision Part A Unit III)*

### Q11. [8 Marks]
a) Explain the **Optimal Binary Search Tree (OBST)** problem. Formulate the recurrence relation for the expected search cost table `e[i, j]` and state its time and space complexity. **[5 Marks]**  
b) What is the **Reliability Design Problem** in Dynamic Programming? Formulate its objective function and describe how multiple redundant stages are evaluated. **[3 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit III Q12(a), Q14(a))*
