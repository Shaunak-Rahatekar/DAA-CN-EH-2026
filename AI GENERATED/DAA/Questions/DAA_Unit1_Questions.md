# DAA Unit I: Examination Questions (Basic Introduction & Complexity Analysis)

> **Course:** CS3212 — Design and Analysis of Algorithms  
> **Source Files Used:**
> - `DAA SYLLABUS.pdf` (Unit I)
> - `DAA Endsem Revision (CS3212).pdf` (Part C, Q1, Q2)
> - `DAAOA_Question_Bank.docx` (Unit I, Q1 to Q19)
> - `DAA_Question_Bank_Ans_Final.docx` (Unit I Model Answers)
>
> *Note:* In accordance with the study system rules, answers are provided in a separate file: [DAA_Unit1_Answers.md](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Questions/DAA_Unit1_Answers.md).

---

## Section A: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
Define the term **algorithm**. State and explain any four essential criteria that a valid algorithm must satisfy.  
*(Source: DAAOA_Question_Bank.docx Unit I Q1)*

### Q2. [4 Marks]
Distinguish clearly between:
1. Algorithm and Pseudocode
2. Time Complexity and Space Complexity  
*(Source: DAAOA_Question_Bank.docx Unit I Q4, Q11(b))*

### Q3. [4 Marks]
Define the three primary asymptotic notations: Big-O (O), Big-Omega (Ω), and Big-Theta (Θ). Provide their formal mathematical inequalities and state the geometric meaning of constants c and n0.  
*(Source: DAAOA_Question_Bank.docx Unit I Q3, Q17)*

### Q4. [4 Marks]
Prove from first principles using the formal definition of Θ that:
```
4n² + 3n = Θ(n²)
```
State explicit values for constants c1, c2, and n0.  
*(Source: DAAOA_Question_Bank.docx Unit I Q19)*

### Q5. [4 Marks]
Define **order of an algorithm**. Why is empirical profiling (measuring clock time in milliseconds) insufficient compared to asymptotic analysis?  
*(Source: DAAOA_Question_Bank.docx Unit I Q2, Q9)*

### Q6. [4 Marks]
Explain the **adversary lower bound technique** for comparison-based sorting. Why can no comparison-based sort beat Ω(n log n) in the worst case?  
*(Source: DAA SYLLABUS.pdf Unit I & Endsem Revision Part C Q2)*

### Q7. [4 Marks]
State what a **loop invariant** is. Describe the three mandatory conditions (initialization, maintenance, termination) required to establish the correctness of an iterative algorithm.  
*(Source: DAA SYLLABUS.pdf Unit I)*

---

## Section B: Long / Numerical Questions (8 to 10 Marks Each)

### Q8. [8 Marks]
a) State the best-case, average-case, and worst-case time complexities of **Binary Search**. Set up the recurrence relation for the worst case and solve it to show that `T(n) = Θ(log n)`. **[4 Marks]**  
b) Explain why Quick Sort exhibits an `O(n²)` worst-case running time. Write its worst-case recurrence relation, explain the scenario that causes it, and derive its solution using repeated substitution. **[4 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit I Q5, Q6, Q8, Q12)*

### Q9. [10 Marks]
a) Write the divide-and-conquer algorithm for **Merge Sort**. Set up its recurrence relation and solve it using the Master Theorem to show that its worst-case complexity is `Θ(n log n)`. **[5 Marks]**  
b) Trace the Merge Sort algorithm step-by-step on the following list of numbers, showing all intermediate split and merge stages:  
```
List: [78, 32, 42, 62, 98, 12, 34, 83, 10]
```
**[5 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit I Q13(b), Q15(b))*

### Q10. [8 Marks]
a) Explain **Strassen’s Matrix Multiplication** algorithm. How does it improve upon the standard `O(n³)` matrix multiplication algorithm? State its recurrence relation and compute its asymptotic time complexity. **[5 Marks]**  
b) Using repeated substitution, find the asymptotic tight bound (Θ) for the recurrence equation:  
```
T(n) = T(n - 1) + n,   with T(1) = 1
```
**[3 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit I Q14(a), Q18)*

### Q11. [10 Marks]
a) Differentiate between **Breadth-First Search (BFS)** and **Depth-First Search (DFS)** in terms of data structures used, traversal strategy, edge classification, and time/space complexities. **[5 Marks]**  
b) What is **Topological Sorting**? State the necessary and sufficient condition for a graph to have a topological order. Describe Kahn’s algorithm (in-degree method) for topological sorting and state its running time. **[5 Marks]**  
*(Source: DAA SYLLABUS.pdf Unit I & general knowledge)*
