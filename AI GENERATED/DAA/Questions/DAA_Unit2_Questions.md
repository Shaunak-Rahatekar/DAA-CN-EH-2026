# DAA Unit II: Examination Questions (Recursion & Divide and Conquer)

> **Course:** CS3212 — Design and Analysis of Algorithms  
> **Source Files Used:**
> - `DAA SYLLABUS.pdf` (Unit II)
> - `DAA Endsem Revision (CS3212).pdf` (Part C, Q3, Q4, Q5, Q11, Q20)
> - `DAAOA_Question_Bank.docx` (Unit II)
> - `DAA_Question_Bank_Ans_Final.docx` (Unit II Model Answers)
>
> *Note:* In accordance with the study system rules, answers are provided in a separate file: [DAA_Unit2_Answers.md](file:///D:/2026%20EXAM/AI%20GENERATED/DAA/Questions/DAA_Unit2_Answers.md).

---

## Section A: Short Answer Questions (4 Marks Each)

### Q1. [4 Marks]
State the **Master Theorem** for divide-and-conquer recurrences. Using the Master Theorem, solve the following recurrence:
```
T(n) = 3T(n/4) + n log n
```
*(Source: DAA Endsem Revision (CS3212).pdf Part C Q3)*

### Q2. [4 Marks]
State the recurrence relation for the **Josephus Problem**. Using the binary representation method, compute the survivor position `J(41)`.  
*(Source: DAA Endsem Revision (CS3212).pdf Part C Q5)*

### Q3. [4 Marks]
Using **Fast Modular Exponentiation** (square-and-multiply method), compute the exact value of:
```
7¹³ mod 11
```
Show every intermediate calculation step and the bits of the exponent.  
*(Source: DAA Endsem Revision (CS3212).pdf Part C Q4)*

### Q4. [4 Marks]
Define **Inversion** in an array. Explain how Merge Sort can be modified to count all inversions in `Θ(n log n)` time without exceeding the standard sorting runtime.  
*(Source: DAA SYLLABUS.pdf Unit II & general knowledge)*

### Q5. [4 Marks]
Differentiate between **Randomized Quickselect** and the deterministic **Median-of-Medians** algorithm for finding the `k`-th smallest element. Compare their worst-case and expected time complexities.  
*(Source: DAA SYLLABUS.pdf Unit II)*

### Q6. [4 Marks]
Explain the **Union by Rank** and **Path Compression (Collapsing Rule)** heuristics in Disjoint Set Union (DSU). State the amortized complexity of `m` operations on `n` elements when both heuristics are used.  
*(Source: DAAOA_Question_Bank.docx Unit II Q8, Q16)*

---

## Section B: Long / Numerical Questions (8 to 10 Marks Each)

### Q7. [10 Marks]
a) Explain **Karatsuba’s Integer Multiplication** algorithm. Show how it reduces the number of sub-problem multiplications from 4 to 3. Derive its recurrence relation and compute its time complexity using the Master Theorem. **[5 Marks]**  
b) Multiply the two 4-digit numbers `1234` and `5678` using Karatsuba’s multiplication technique, clearly showing the three sub-products `P1`, `P2`, `P3` and the combination step. **[5 Marks]**  
*(Source: DAA Endsem Revision (CS3212).pdf Part C Q20)*

### Q8. [10 Marks]
a) Describe the Divide-and-Conquer algorithm for finding the **Closest Pair of Points** in a 2D plane. Explain the geometric rationale behind why each point in the vertical strip needs to be compared with at most 7 other points. **[6 Marks]**  
b) Set up the recurrence relation for the 2D Closest Pair algorithm and solve it to show that its total time complexity is `Θ(n log n)`. **[4 Marks]**  
*(Source: DAA SYLLABUS.pdf Unit II & CLRS Chapter 33)*

### Q9. [8 Marks]
a) Explain **Quick Sort** and its **Randomized** version. Derive its best-case, average-case, and worst-case time complexities. Why does randomizing the pivot eliminate the worst-case trigger for sorted arrays? **[5 Marks]**  
b) You have Quick Sort and Merge Sort available. For the following data scenarios, choose which sorting algorithm you would recommend and justify your selection:  
   1. Data is already sorted in ascending order.  
   2. System has severe memory constraints (cannot allocate `O(n)` extra memory).  
   3. The data is stored as a Singly Linked List. **[3 Marks]**  
*(Source: DAAOA_Question_Bank.docx Unit II Q17 & Endsem Revision Part C Q11)*

### Q10. [8 Marks]
a) Explain the **Tower of Hanoi** puzzle. Write the recursive algorithm to move `n` disks from peg A to peg C using peg B. Derive its recurrence relation and prove that the total number of moves required is `2ⁿ - 1`. **[5 Marks]**  
b) Explain the **Boyer-Moore Voting Algorithm** for finding the majority element in an array. Trace it on the array:  
```
[2, 2, 1, 1, 1, 2, 2]
```
State its time and auxiliary space complexity. **[3 Marks]**  
*(Source: DAA SYLLABUS.pdf Unit II & general knowledge)*
