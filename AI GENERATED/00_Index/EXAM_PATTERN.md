# EXAM PATTERN & QUESTION BANK ANALYSIS

> Last updated: 20 Nov 2026
> Sources:
> - `DAA SYLLABUS.pdf` (CS3212)
> - `DAA Endsem Revision (CS3212).pdf`
> - `DAAOA_Question_Bank.docx` (Department Question Bank)
> - `DAA_Question_Bank_Ans_Final.docx` (Department Model Answers, 230k+ chars)
> - `CN SYLLABUS.pdf` & `IT2266_CN-ESE-QUESTION-PAPER.pdf`
> - `ETHICAL HACKING SYLLABUS.pdf` & `Ethical Hacking Endsem Revision (MD ).pdf`

---

## 1. DAA (CS3212) — Deep-Dive Unit-Wise Analysis

The newly extracted `.docx` files reveal the exact structure, questions, and mark-allocation style used by the department examiners.

### Exam Characteristics
- **Format:** Mix of short definition questions (2–4 marks) and comprehensive 8–10 mark questions (derivations, step-by-step algorithms, trace tables, state-space trees, and proofs).
- **Style:** Every 10-mark question requires:
  1. Formal Definition & Properties
  2. Algorithm / Pseudocode / Recurrence Relation
  3. Step-by-step Traced Example / Table / State-Space Tree
  4. Time & Space Complexity Derivation (Best, Worst, Average)

---

### Unit I: Basic Introduction & Time/Space Complexity Analysis

- **Syllabus Topics:** Asymptotic notations (Big-O, Ω, Θ, small-o), Best/Worst/Avg case, Analysis of recursive & non-recursive algorithms, BFS/DFS, Topological Sort, Adversary lower bound for comparison sorting ($\Omega(n \log n)$), Loop invariants & correctness proofs.
- **Direct Question Bank Highlights:**
  - *Short / Concepts:* Define algorithm, 5 criteria of good algorithm (Input, Output, Finiteness, Definiteness, Effectiveness). Algorithm vs Pseudocode. Order of algorithm and need for analysis.
  - *Asymptotic Notations:* Formal definitions with bounding constants ($c_1, c_2, n_0$). Proof that $4n^2 + 3n = \Theta(n^2)$ and $3n^2 + 5n = \Theta(n^2)$.
  - *Algorithm Analysis:*
    - Binary Search: Best case $O(1)$, Worst case $O(\log n)$ (with recurrence $T(n) = T(n/2) + 1$).
    - Quick Sort: Best case $O(n \log n)$, Worst case $O(n^2)$ recurrence $T(n) = T(n-1) + n$, Average case derivation $O(n \log n)$.
    - Merge Sort: Worst case recurrence $T(n) = 2T(n/2) + \Theta(n) \to O(n \log n)$.
    - Recurrence solving: Iterative/Substitution for $T(n) = T(n-1) + n \to \Theta(n^2)$.
    - Strassen's Matrix Multiplication: 7 multiplications instead of 8, recurrence $T(n) = 7T(n/2) + O(n^2) \to O(n^{\log_2 7}) = O(n^{2.81})$.
    - Tracing Merge Sort on numeric list: `[78, 32, 42, 62, 98, 12, 34, 83, 10]`.

---

### Unit II: Divide and Conquer & Combinatorial Search

- **Syllabus Topics:** D&C strategy, Recurrences & Master Theorem, Combinatorial enumeration, Josephus problem, Tower of Hanoi, Quick sort, Merge sort, Counting inversions, Majority element, Order statistics, Closest pair of points, Integer arithmetic (Euclid GCD, Karatsuba, Fast modular exponentiation).
- **Direct Question Bank Highlights:**
  - *Set Operations & Disjoint Sets:* Union and Find algorithms, Weighted Union (Union by Rank/Size), Collapsing Find (Path Compression), Connected & Bi-connected components.
  - *Algorithm Selection:* Criteria for choosing Quick Sort vs Merge Sort for specific input types (nearly sorted, random, limited memory, linked lists).
  - *Backtracking Foundations (connected with D&C in QB):*
    - 4-Queens Problem: Formulation and drawing the complete state-space tree.
    - Hamiltonian Cycle & Path: Backtracking procedure and state-space tree.
    - Sum of Subsets: Fixed-tuple approach vs variable-tuple approach, worked example for $n=4, w=(11, 13, 24, 7), M=31$.
    - Graph Coloring: Chromatic number, state-space tree for $m=3$ colors, $n=4$ vertices, time complexity $O(m^n)$.
    - Divide and Conquer vs Greedy vs Dynamic Programming comparison tables.

---

### Unit III: Dynamic Programming & Greedy Algorithms

- **Syllabus Topics:** Principle of optimality, Overlapping subproblems, Matrix Chain Multiplication, Coin Change, TSP (Held-Karp), OBST, Shortest paths in DAG, Floyd-Warshall APSP, LIS, LCS, Largest Independent Set in trees.
- **Direct Question Bank Highlights:**
  - *Core Concepts:* Definition of Dynamic Programming, Principle of Optimality, Comparison: DP vs Greedy vs Divide & Conquer.
  - *0/1 Knapsack Problem (DP):* Tabulation formula $K[i, w] = \max(K[i-1, w], v_i + K[i-1, w-w_i])$, full matrix computation for capacity $W=7$.
  - *Matrix Chain Multiplication (MCM):*
    - Formula: $m[i,j] = \min_{i \le k < j} \{m[i,k] + m[k+1,j] + p_{i-1}p_k p_j\}$.
    - Worked problem with dimensions: $A_1(3 \times 2), A_2(2 \times 4), A_3(4 \times 2), A_4(2 \times 5)$, computing $m$-table and $s$-table, optimal parenthesization.
  - *Optimal Binary Search Tree (OBST):* Recurrence $e[i,j] = \min_r \{e[i, r-1] + e[r+1, j] + w[i,j]\}$, table construction.
  - *All-Pairs Shortest Path (Floyd-Warshall):* Formula $D^k[i,j] = \min(D^{k-1}[i,j], D^{k-1}[i,k] + D^{k-1}[k,j])$, step-by-step matrix updates for $k=1 \dots n$.
  - *Travelling Salesperson Problem (TSP):* Held-Karp recurrence $C(S, j) = \min_{i \in S - \{j\}} \{C(S - \{j\}, i) + d_{ij}\}$, time complexity $O(n^2 2^n)$.
  - *Reliability Design Problem:* Stage-by-stage dynamic programming optimization.

---

### Unit IV: Greedy Strategy & Backtracking

- **Syllabus Topics:** Greedy strategy, MST (Kruskal, Prim), Dijkstra, Fractional Knapsack, Conflict-free scheduling, Huffman coding, Backtracking (N-queens, graph coloring, Hamiltonian cycle, subset sum, Sudoku).
- **Direct Question Bank Highlights:**
  - *Definitions:* Feasible solution, Optimal solution, Greedy choice property, Optimal substructure.
  - *Minimum Spanning Tree (MST):*
    - Prim's Algorithm: Stepwise tree growth, priority queue implementation, time $O(E \log V)$.
    - Kruskal's Algorithm: Edge sorting, Disjoint Set Union (DSU) cycle detection, time $O(E \log E)$.
    - Comparison: Prim vs Kruskal (dense vs sparse graphs).
  - *Shortest Path:* Dijkstra's Algorithm (worked example, non-negative edge requirement, proof of optimality).
  - *Fractional Knapsack:* Value-to-weight ratio greedy choice, proof of why greedy fails for 0/1 Knapsack.
  - *Job Sequencing with Deadlines:*
    - Formulation, slot allocation, optimal job sequence and maximum profit calculation for $n=5$ jobs.
  - *Conflict-Free Activity Scheduling:* Greedy selection sorted by finish times.
  - *Huffman Coding:* Min-heap construction, prefix-free codes, average code length calculation.

---

### Unit V: Complexity Classes & NP-Completeness

- **Syllabus Topics:** Complexity classes P, NP, coNP, Polynomial time reduction ($A \le_p B$), NP-hard, NP-complete, Cook-Levin Theorem, Halting problem, Reductions (SAT $\to$ 3-SAT $\to$ Clique $\to$ Vertex Cover $\to$ Independent Set), Integer Linear Programming (ILP) modelling.
- **Direct Question Bank Highlights:**
  - *Definitions:* Deterministic vs Non-Deterministic algorithms (Choice, Evaluation, Success). Class P, Class NP, NP-Hard, NP-Complete.
  - *Cook-Levin Theorem:* Statement, historical importance (SAT is the first NP-Complete problem), implications for $P$ vs $NP$.
  - *Strategy to Prove NP-Completeness (3-Step Template):*
    1. Show problem $L \in \text{NP}$ (polynomial-time certificate verification).
    2. Choose a known NP-Complete problem $L'$.
    3. Construct polynomial-time reduction $L' \le_p L$ and prove bi-directional equivalence ($x \in L' \iff f(x) \in L$).
  - *Branch and Bound (BB):*
    - Difference between Backtracking (DFS) and Branch & Bound (BFS/Priority Search).
    - LC-Search (Least Cost Search), FIFO-BB, Bounding functions.
    - 0/1 Knapsack using FIFO-BB and LC-BB (state-space tree with upper/lower bounds).
    - TSP using LC-BB (Reduced cost matrix method).

---

### Unit VI: Randomized & Approximation Algorithms (and Network Flows)

- **Syllabus Topics:**
  - Network Flow (Ford-Fulkerson, residual network, cuts, Max-Flow Min-Cut, bipartite matching).
  - Randomized Algorithms: Las Vegas (always correct, random time) vs Monte Carlo (fixed time, bounded error probability), Error amplification by repetition, Abundance of witnesses, Randomized QuickSort, Karger's Min-Cut algorithm, Coupon Collector problem.
  - Approximation Algorithms: Approximation ratio $\rho(n)$, 2-approximation for Vertex Cover (maximal matching heuristic), 2-approximation for Metric-TSP (MST doubling + shortcutting), Hardness of approximation (general TSP is not approximable in polynomial time unless $P=NP$).
  - Quantum Computation Basics (in QB): Qubits, superposition, entanglement, quantum impact on classical algorithms (Shor's factoring algorithm $O((\log N)^3)$, Grover's search $O(\sqrt{N})$).

---

## 2. Summary of High-Yield Repeated Question Types Across DAA

| Category | Must-Master Topics | Typical Marks |
|----------|-------------------|:-------------:|
| **Recurrences & Analysis** | Master Theorem, QuickSort best/worst/avg derivation, MergeSort recurrence | 4 – 10 M |
| **Tabular DP Problems** | 0/1 Knapsack table ($W=7$), Matrix Chain Multiplication ($A_1 \dots A_4$), LCS table | 8 – 10 M |
| **Greedy Tracing** | Kruskal with DSU, Prim with Priority Queue, Dijkstra shortest path, Job Sequencing with deadlines | 8 – 10 M |
| **Backtracking Trees** | 4-Queens state-space tree, Sum of Subsets ($M=31$), Graph 3-Coloring tree | 8 – 10 M |
| **NP-Completeness** | Definitions (P, NP, NP-Hard, NP-Complete), Cook-Levin statement, 3-SAT $\to$ Clique reduction, BB for Knapsack/TSP | 4 – 10 M |
| **Randomized / Approx** | Las Vegas vs Monte Carlo, Karger's Min-Cut, 2-approx Vertex Cover, 2-approx Metric-TSP | 4 – 10 M |

---

## 3. CN & EH Exam Pattern Quick Recap

- **CN (26 Nov):** Heavy numerical content (Hamming code, CRC division, sliding window efficiency $1/(1+2a)$, subnetting $/24 \to /27$, CIDR, TCP cwnd trace) + standard protocols (OSI vs TCP/IP, CSMA/CD backoff, Distance Vector, DNS, DHCP DORA).
- **EH (27 Nov):** Pure theory & analytical marks (CIA triad, Cyber Kill Chain, OWASP Top 10 2025 vs 2021, SQLi/XSS/CSRF prevention, WEP/WPA/WPA2/WPA3 comparison table, Incident Response lifecycle, Case studies: WannaCry, Equifax, Log4Shell, SolarWinds).
