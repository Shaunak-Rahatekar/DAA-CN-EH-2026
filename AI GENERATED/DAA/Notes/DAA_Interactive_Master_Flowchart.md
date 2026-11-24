# DAA (CS3212 / CS3205) — Master Interactive Exam Flowchart & Case-Study Blueprint

> **Exam Target**: VIT Pune End-Semester Examination (60 Marks, 2 Hours)  
> **Coverage**: Units I – VI | High-Yield Topics | CLRS 3rd Ed. Problem Mapping | Case-Study Answering Templates  
> **Key Features**: Instant Hinglish 1-Sec Intuition + Standard English Definitions + Algorithm Design Decoders + 4-Section Scoring Blueprints  

---

## 🗺️ Master Visual Flowchart (High-Level Architecture)

```
===================================================================================================
                                  DESIGN & ANALYSIS OF ALGORITHMS (CS3212)
===================================================================================================
   │
   ├── [UNIT I: COMPLEXITY & RECURRENCES]
   │    ├── 1.1 Asymptotic Notations & Limit Theorems
   │    ├── 1.2 Recurrences: Master Theorem & Variable Substitution (Sqrt Recurrence)
   │    ├── 1.3 Loop Invariants & Iterative Analysis
   │    ├── 1.4 Decision Tree Lower Bound for Sorting (Omega(n log n))
   │    └── 1.5 Strassen's Matrix Multiplication
   │
   ├── [UNIT II: DIVIDE & CONQUER & COMBINATORIAL SEARCH]
   │    ├── 2.1 Counting Inversions & Merge Sort
   │    ├── 2.2 Order Statistics: Median-of-Medians (Deterministic O(n)) & k-Array Median
   │    ├── 2.3 Computational Geometry: Closest Pair of Points (2D Strip Check)
   │    ├── 2.4 Fast Modular Exponentiation & Euclid GCD
   │    ├── 2.5 Josephus Problem & Binary Left-Shift Invariant
   │    └── 2.6 Disjoint Set Union (DSU) / Union-Find with Path Compression
   │
   ├── [UNIT III: DYNAMIC PROGRAMMING]
   │    ├── 3.1 0/1 Knapsack Problem (Tabulation & Traceback)
   │    ├── 3.2 Matrix Chain Multiplication (MCM) & Catalan Parenthesizations
   │    ├── 3.3 Longest Common Subsequence (LCS) & Palindrome Insertions
   │    ├── 3.4 Longest Arithmetic Progression Subsequence (LAPS)
   │    ├── 3.5 All-Pairs Shortest Paths (Floyd-Warshall) & Length-Bounded Directed Paths
   │    ├── 3.6 Poisson Binomial Distribution DP (Probability of k Heads)
   │    └── 3.7 Optimal Binary Search Trees (OBST)
   │
   ├── [UNIT IV: GREEDY STRATEGY & BACKTRACKING]
   │    ├── 4.1 Huffman Coding & Optimal Prefix Trees (Verbatim ESE Question)
   │    ├── 4.2 Minimum Spanning Trees (Prim vs Kruskal & Cut Property)
   │    ├── 4.3 Single-Source Shortest Paths (Dijkstra's Greedy Relaxation)
   │    ├── 4.4 Backtracking: 4-Queens / N-Queens State-Space Tree
   │    ├── 4.5 Backtracking: Sum of Subsets, Graph m-Coloring & Sudoku
   │    └── 4.6 Greedy / DP on Trees (Maximum Independent Set / Blue Vertices)
   │
   ├── [UNIT V: NETWORK FLOW OPTIMIZATION]
   │    ├── 5.1 Flow Network Axioms & Residual Graphs
   │    ├── 5.2 Ford-Fulkerson & Edmonds-Karp BFS Shortest Augmenting Path
   │    ├── 5.3 Max-Flow Min-Cut Theorem & Capacity Verification
   │    └── 5.4 Maximum Bipartite Matching Reduction
   │
   └── [UNIT VI: NP-COMPLETENESS, APPROXIMATION & RANDOMIZATION]
        ├── 6.1 Complexity Hierarchy: P, NP, NP-Hard, NP-Complete (Prover-Verifier)
        ├── 6.2 Polynomial Reductions: 3-SAT to CLIQUE & VERTEX COVER
        ├── 6.3 Undecidability: Halting Problem Diagonalization Proof
        ├── 6.4 2-Approximations: Vertex Cover (Maximal Matching) & Metric TSP (MST Doubling)
        ├── 6.5 Randomized Algorithms: Monte Carlo vs Las Vegas (Popular Element >= n/3)
        └── 6.6 Discrete Geometry: Planar Red-Blue Point Bisection & Non-Crossing Matching
===================================================================================================
```

---

## 🏛️ The Universal 4-Section Examiner Scoring Rubric
Every time a question says: *"Design an efficient algorithm for..."*, structure your answer into these 4 titled sections:

1. **Section 1: Paradigm & State Definition (1 Mark)**  
   *Explicitly state the paradigm (e.g., Dynamic Programming, Divide & Conquer, Las Vegas).*  
   *Define the mathematical state: "Let $DP[i][j]$ represent..."*
2. **Section 2: Recurrence Relation & Base Cases (2 Marks)**  
   *Write the mathematical recurrence equation and boundary base conditions.*
3. **Section 3: Procedural Pseudocode (2-3 Marks)**  
   *Write clear 1D/2D array procedural code with loops. No high-level abstractions or external libraries.*
4. **Section 4: Correctness Proof & Asymptotic Complexity (2 Marks)**  
   *Correctness: 2-3 sentences of inductive invariant reasoning.*  
   *Time Complexity: State table dimension and inner work per cell.*  
   *Space Complexity: State array storage requirements.*

---

# ===================================================================
# UNIT I: COMPLEXITY ANALYSIS & RECURRENCES
# ===================================================================

## 1.1 Asymptotic Notations & Limit Definitions
* 💡 **Hinglish "Asli Baat"**: $O$ matlab "isse bura nahi hoga" (upper ceiling), $\Omega$ matlab "isse better nahi ho sakta" (lower floor), aur $\Theta$ matlab "exact yahi rate of growth hai" (sandwich bound). Small-o aur small-$\omega$ strict inequality hote hain jahan curve touch bhi nahi kar sakti.
* 🎯 **Standard English Definition**: Mathematical bounds characterizing algorithm growth rate as input $n \to \infty$. $f(n) = \Theta(g(n)) \iff \exists c_1, c_2 > 0, n_0 \ge 1$ such that $0 \le c_1 g(n) \le f(n) \le c_2 g(n)$ for all $n \ge n_0$.
* 📐 **Core Formulas & Limit Tests**:
  * $\lim_{n \to \infty} \frac{f(n)}{g(n)} = c \in (0, \infty) \implies f(n) = \Theta(g(n))$
  * $\lim_{n \to \infty} \frac{f(n)}{g(n)} = 0 \implies f(n) = o(g(n))$ and $f(n) = O(g(n))$
  * $\lim_{n \to \infty} \frac{f(n)}{g(n)} = \infty \implies f(n) = \omega(g(n))$ and $f(n) = \Omega(g(n))$
* ❓ **Question Typology**:
  1. *Formal Definition with diagram*: Define $O, \Omega, \Theta, o, \omega$ with constants $c, n_0$ and draw curves.
  2. *Verification by constants*: Find $c_1, c_2, n_0$ for $4n^2 + 3n = \Theta(n^2)$.
  3. *Function comparison*: Classify $f(n) = n^{\log n}$ vs $g(n) = 2^n$ (Hint: $n^{\log n} = 2^{\log^2 n} = o(2^n)$).
* 🛠️ **Algorithm Design & Case-Study Decoder Table**:
  | Scenario / Real-World Trigger | Secret Paradigm | State / Variable Notation | Mathematical Invariant | 4-Section Answer Blueprint |
  | :--- | :--- | :--- | :--- | :--- |
  | Comparing asymptotic ranking of exotic functions | Limit Calculus & Log Transform | Let $L = \lim_{n \to \infty} \frac{\ln f(n)}{\ln g(n)}$ | L'Hôpital's Rule: $\frac{f'(n)}{g'(n)}$ | 1. State limit definition<br>2. Take natural logarithm of both functions<br>3. Compute limit as $n \to \infty$<br>4. Conclude strict or tight bound |
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 3, Problem 3-2 (Relative asymptotic growths)**: Compare $n^k$ vs $c^n$, $2^n$ vs $2^{n/2}$.
  * **Actual Paper 1, Q1(a)**: Classify (i) $f(n) = n^{\log n}, g(n) = 2^n \implies f(n) = o(g(n))$; (ii) $f(n) = \log(\sqrt{n}/5), g(n) = \log n \implies f(n) = \Theta(g(n))$.

---

## 1.2 Recurrences: Master Theorem & Variable Substitution
* 💡 **Hinglish "Asli Baat"**: Master theorem me comparison $n^{\log_b a}$ (leaves ka weight) aur $f(n)$ (root ka split cost) ke beech hota hai. Jo dominating hoga wahi final answer banega. Agar square root ($\sqrt{n}$) dikhe, toh $n = 2^m$ daal kar binary search me convert kar lo!
* 🎯 **Standard English Definition**: Master Theorem solves recurrences of the form $T(n) = a T(n/b) + f(n)$ by comparing $f(n)$ to the watershed function $n^{\log_b a}$. Variable substitution transforms non-standard forms into linear recurrences.
* 📐 **Core Formulas**:
  * **Master Theorem**: Critical exponent $p = \log_b a$.
    * *Case 1*: $f(n) = O(n^{p - \epsilon}) \implies T(n) = \Theta(n^p)$.
    * *Case 2*: $f(n) = \Theta(n^p \log^k n) \implies T(n) = \Theta(n^p \log^{k+1} n)$.
    * *Case 3*: $f(n) = \Omega(n^{p + \epsilon})$ and regularity $a f(n/b) \le c f(n) \implies T(n) = \Theta(f(n))$.
  * **Square Root Substitution**: For $T(n) = T(\sqrt{n}) + 1$:
    * Let $n = 2^m \implies m = \log_2 n$.
    * $S(m) = T(2^m) = T(2^{m/2}) + 1 = S(m/2) + 1 \implies S(m) = \Theta(\log m) \implies T(n) = \mathbf{\Theta(\log \log n)}$.
* ❓ **Question Typology**:
  1. *Direct Master Theorem*: Solve $T(n) = 4T(n/2) + n^2 \implies \text{Case 2} \implies \Theta(n^2 \log n)$.
  2. *Substitution on Square Root*: Solve $T(n) = 2T(\sqrt{n}) + \log n \implies O(\log n \log \log n)$.
  3. *Unbalanced Split*: Solve $T(n) = T(n/3) + T(2n/3) + n \implies \Theta(n \log n)$ via Recursion Tree.
* 🛠️ **Algorithm Design & Case-Study Decoder Table**:
  | Scenario / Real-World Trigger | Secret Paradigm | State / Variable Notation | Mathematical Invariant | 4-Section Answer Blueprint |
  | :--- | :--- | :--- | :--- | :--- |
  | Algorithm splits data into halves, but subproblem size shrinks by square root | Domain Transformation via Substitution | $n = 2^m, m = \log_2 n$<br>$S(m) = T(2^m)$ | $S(m) = a S(m/2) + g(m)$ | 1. Identify non-linear domain shrinkage<br>2. Substitute $n = 2^m$<br>3. Apply Master Theorem on $S(m)$<br>4. Back-substitute $m = \log n$ |
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 4, Section 4.5 & Exercise 4.5-3**: $T(n) = 4T(n/2) + n^2$ and $T(n) = 3T(n/2) + n$.
  * **Actual Paper 2, Q1(a)**: $T(n) = T(\sqrt{n}) + 1 \implies \mathbf{O(\log \log n)}$; $T(n) = 3T(n/2) + n \implies \mathbf{O(n^{\log_2 3})}$.

---

## 1.3 Decision Tree Lower Bound for Comparison Sorting
* 💡 **Hinglish "Asli Baat"**: Kisi bhi comparison-based sorting algorithm ko $n$ elements sort karne ke liye kam se kam $\Omega(n \log n)$ comparisons karne hi padenge, kyunki $n!$ possible answers ko distinguish karne ke liye binary tree ki height kam se kam $\log_2(n!)$ honi padegi.
* 🎯 **Standard English Definition**: In a comparison sort, any algorithm can be represented as an adversary decision tree where internal nodes are comparisons $A[i] \le A[j]$ and leaves are permutations. To sort $n$ items, the tree must have at least $n!$ leaves. Height $h \ge \log_2(n!) = \Omega(n \log n)$.
* 📐 **Core Proof by Stirling's Approximation**:
  $$L \ge n! \implies h \ge \log_2(n!) = \sum_{i=1}^n \log_2 i \ge \sum_{i=n/2}^n \log_2(n/2) = \frac{n}{2} \log_2(n/2) = \mathbf{\Omega(n \log n)}$$
* ❓ **Question Typology**:
  1. *Adversary Lower Bound Proof*: Draw decision tree for $n=3$, label internal nodes and 6 leaves, prove $h = \Omega(n \log n)$.
  2. *Linear Time Sorting Exceptions*: Why can Counting Sort / Radix Sort run in $O(n)$? (Answer: Non-comparison sorts don't use decision trees).
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 8, Section 8.1**: Lower bounds for sorting (Theorem 8.1).

---

## 1.4 Strassen's Matrix Multiplication
* 💡 **Hinglish "Asli Baat"**: Do $n \times n$ matrices multiply karne me normal method 8 multiplications leta hai ($O(n^3)$). Strassen ne clever algebraic grouping se multiplications ko 8 se ghata kar 7 kar diya, jisse complexity $O(n^{\log_2 7}) = O(n^{2.81})$ ho gayi!
* 🎯 **Standard English Definition**: A divide-and-conquer matrix multiplication method that computes the product of two $2 \times 2$ block submatrices using only 7 multiplications and 18 additions/subtractions, yielding $T(n) = 7 T(n/2) + O(n^2) = \Theta(n^{\log_2 7}) \approx O(n^{2.81})$.
* 📐 **Core Recurrence**:
  $$T(n) = 7 T(n/2) + \Theta(n^2) \implies a=7, b=2, f(n)=n^2 \implies n^{\log_2 7} \approx n^{2.807} \implies T(n) = \mathbf{\Theta(n^{2.81})}$$
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 4, Section 4.2**: Strassen's algorithm for matrix multiplication.

---

# ===================================================================
# UNIT II: DIVIDE & CONQUER & COMBINATORIAL SEARCH
# ===================================================================

## 2.1 Counting Inversions in an Array
* 💡 **Hinglish "Asli Baat"**: Inversion ka matlab array me kitne elements ulte order me hain ($i < j$ lekin $A[i] > A[j]$). Merge sort ke doran jab right half se chhota element pehle uthate hain, toh left half ke bache saare elements usse bade hote hain — wahi instant inversion count ban jata hai!
* 🎯 **Standard English Definition**: Measures how far an array is from being sorted. Count pairs $(i, j)$ such that $i < j$ and $A[i] > A[j]$. Solved in $O(n \log n)$ by piggybacking on Merge Sort's combine step.
* 🛠️ **Algorithm Design & Case-Study Decoder Table**:
  | Scenario / Real-World Trigger | Secret Paradigm | State / Variable Notation | Mathematical Invariant | 4-Section Answer Blueprint |
  | :--- | :--- | :--- | :--- | :--- |
  | Measuring similarity between two user rankings or collaborative filtering | Divide & Conquer (Enhanced Merge Sort) | Count split inversions during merge of $A[p..q]$ and $A[q+1..r]$ | If $R[j] < L[i]$, all remaining elements $L[i..n_1]$ form inversions with $R[j]$: $\text{count} += (n_1 - i + 1)$ | 1. State D&C approach: `Inversions(L) + Inversions(R) + SplitInv`<br>2. Recurrence: $T(n) = 2T(n/2) + O(n) = O(n \log n)$<br>3. Pseudocode with two-pointer merge<br>4. Prove every inverted pair is counted exactly once |
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 2, Problem 2-4 (Inversions)**: Full problem statement and $O(n \log n)$ merge sort adaptation.

---

## 2.2 Median of $k$ Sorted Arrays & Order Statistics
* 💡 **Hinglish "Asli Baat"**: Agar $k$ sorted arrays diye hain jinka total size $n$ hai, toh har array ke pehle element ko Min-Heap me daal do (size $k$). Heap se chhota element nikaalo aur usi array ka agla element heap me bharte jao. $n/2$ steps me exact median mil jayega in $O(n \log k)$!
* 🎯 **Standard English Definition**: Finding the $\lfloor n/2 \rfloor$-th smallest element across $k$ disjoint sorted arrays $A_1, \dots, A_k$. Using a size-$k$ min-heap priority queue, extract minimum $n/2$ times in $O(n \log k)$ time and $O(k)$ space.
* 🛠️ **Algorithm Design & Case-Study Decoder Table**:
  | Scenario / Real-World Trigger | Secret Paradigm | State / Variable Notation | Mathematical Invariant | 4-Section Answer Blueprint |
  | :--- | :--- | :--- | :--- | :--- |
  | Distributed database query returning top/median values from $k$ server shards | Heap / Tournament Selection | Heap contains tuple `(value, array_index, element_index)` of size $k$ | At step $m$, extracted element is the global $m$-th order statistic of $\bigcup A_i$ | 1. Name paradigm: Min-Heap Tournament Selection<br>2. Base setup: Insert first element of each of the $k$ arrays<br>3. Pseudocode: Loop $m = 1 \dots \lfloor n/2 \rfloor$: `Extract-Min`, insert next element<br>4. Time: $O(k + n \log k)$, Space: $O(k)$ |
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 6, Exercise 6.5-9**: Merge $k$ sorted lists using min-heap in $O(n \log k)$.
  * **Actual Paper 2, Q6(a)**: Find median of $k$ sorted arrays in $O(n \log k)$ with proper pseudocode.

---

## 2.3 Computational Geometry: 2D Closest Pair of Points
* 💡 **Hinglish "Asli Baat"**: Points ko X-coordinate se sort karke beech me se do hisson me kaat do. Left aur right halves ka minimum distance $\delta$ nikaalo. Ab dividing line ke dono taraf $\delta$ width ki ek vertical strip banao. Is strip me har point ko sirf uske upar ke 7 points se compare karna padta hai!
* 🎯 **Standard English Definition**: Finds two points with smallest Euclidean distance in $O(n \log n)$. Divides by median $x$-coordinate, recursively finds $\delta = \min(\delta_L, \delta_R)$, and checks points in vertical strip $[x_{mid} - \delta, x_{mid} + \delta]$ sorted by $y$-coordinate.
* 📐 **Core Geometric Invariant**:
  Any square of size $\delta \times \delta$ in the strip can contain at most 4 points without violating the definition of $\delta$. Thus, for each point in the strip, we need only compare it against at most **7 subsequent points** in $y$-sorted order!
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 33, Section 33.4**: Finding the closest pair of points.

---

## 2.4 Fast Modular Exponentiation & Josephus Cycle
* 💡 **Hinglish "Asli Baat"**: 
  * *Fast Exp*: $a^b \bmod n$ nikaalne ke liye power ko aadha karte jao: agar $b$ even hai toh $(a^2)^{b/2}$, agar odd hai toh $a \times a^{b-1}$. Sirf $\log b$ steps lagenge!
  * *Josephus*: $n$ logo ke circle me har alternate banda mara ja raha hai. Agar $n = 2^m + l$ hai, toh winner hamesha **$2l + 1$** hota hai (binary number ka Most Significant Bit utha kar aakhri me lagana!).
* 📐 **Core Formulas**:
  * **Fast Mod Exp**:
    $$a^b \bmod n = \begin{cases} 1 & \text{if } b = 0 \\ ((a^2 \bmod n)^{b/2}) \bmod n & \text{if } b \text{ is even} \\ (a \cdot (a^{b-1} \bmod n)) \bmod n & \text{if } b \text{ is odd} \end{cases}$$
  * **Josephus Recurrence**:
    $$f(2k) = 2f(k) - 1, \quad f(2k+1) = 2f(k) + 1 \implies \text{For } n = 2^m + l, \quad \mathbf{W(n) = 2l + 1}$$
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 31, Section 31.6**: Modular exponentiation (`MODULAR-EXPONENTIATION`).
  * **Actual Paper 1, Q2(a) & Q2(b)**: $3^{83} \bmod 17 = 10$; Josephus for $n = 2^m + 1 \implies \mathbf{3}$ and $n = 2^m - 1 \implies \mathbf{2^m - 3}$.

---

# ===================================================================
# UNIT III: DYNAMIC PROGRAMMING
# ===================================================================

## 3.1 Matrix Chain Multiplication & Catalan Parenthesizations
* 💡 **Hinglish "Asli Baat"**: Matrices ko kis order me multiply karein ki total scalar multiplications sabse kam hon? Recurrence me har possible split point $k$ try karo. Aur total parenthesizations ki ginti hamesha **Catalan numbers** hoti hai ($C_{n-1}$), jo exponential $\Omega(2^n)$ hoti hai!
* 🎯 **Standard English Definition**: DP algorithm determining optimal parenthesis order for multiplying $n$ matrices $A_1 \dots A_n$ of dimensions $p_0 \dots p_n$ in $O(n^3)$ time and $O(n^2)$ space. The count of valid parenthesizations satisfies the Catalan recurrence.
* 📐 **Core Recurrences & Catalan Series**:
  * **Minimum Multiplications DP**:
    $$m[i, j] = \begin{cases} 0 & \text{if } i = j \\ \min_{i \le k < j} \{m[i, k] + m[k+1, j] + p_{i-1}p_k p_j\} & \text{if } i < j \end{cases}$$
  * **Parenthesization Counting (Catalan Recurrence)**:
    $$f(n) = \sum_{k=1}^{n-1} f(k) f(n-k) \quad \text{with } f(1) = 1 \implies f(n) = C_{n-1} = \frac{1}{n}\binom{2n-2}{n-1}$$
  * **Essential Catalan Sequence**:
    $$C_0=1, C_1=1, C_2=2, C_3=5, C_4=14, C_5=42, \mathbf{C_6=132}, \mathbf{C_7=429}$$
    * *Notice*: $f(7) = C_6 = 132$, and $f(8) = C_7 = 429$.
* 🛠️ **Algorithm Design & Case-Study Decoder Table**:
  | Scenario / Real-World Trigger | Secret Paradigm | State / Variable Notation | Mathematical Invariant | 4-Section Answer Blueprint |
  | :--- | :--- | :--- | :--- | :--- |
  | Counting ways to evaluate expressions or parse syntax trees without associativity | Catalan Combinatorial Counting | Let $f(n)$ be parenthesizations of $n$ symbols | Root binary operation splits expression into $k$ and $n-k$ parts | 1. Define split point $k$<br>2. Formulate Catalan convolution sum<br>3. Evaluate base cases: $f(1)=1, f(2)=1, f(3)=2$<br>4. Prove $f(n) \ge 2f(n-1) \implies \Omega(2^n)$ by induction |
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 15, Section 15.2**: Matrix-chain multiplication (Recurrence 15.6, Theorem 15.1).
  * **Actual Paper 2, Q1(b)**: Matrix Chain parenthesizations, evaluate $f(7)=132, f(8)=429$, and prove $\Omega(2^n)$.

---

## 3.2 Longest Common Subsequence & Minimum Insertions for Palindrome
* 💡 **Hinglish "Asli Baat"**: 
  * *LCS*: Do strings me longest common order dhoondhna — match mile toh $(i-1, j-1) + 1$, varna $\max(\text{top}, \text{left})$.
  * *Palindrome Insertions*: String ko palindrome banane ke liye kitne characters add karein? Answer simple hai: **$\text{Total Length } n - \text{LCS}(s, s^{\text{reverse}})$**! Ya direct interval DP chala do.
* 📐 **Core Recurrences**:
  * **LCS Recurrence**:
    $$c[i, j] = \begin{cases} 0 & \text{if } i=0 \text{ or } j=0 \\ c[i-1, j-1] + 1 & \text{if } x_i = y_j \\ \max(c[i-1, j], c[i, j-1]) & \text{if } x_i \ne y_j \end{cases}$$
  * **Palindrome Insertion Interval DP**:
    $$DP[i][j] = \begin{cases} 0 & \text{if } i \ge j \\ DP[i+1][j-1] & \text{if } s[i] == s[j] \\ 1 + \min(DP[i+1][j], DP[i][j-1]) & \text{if } s[i] \ne s[j] \end{cases}$$
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 15, Section 15.4 & Exercise 15.4-4**: Longest common subsequence.
  * **Actual Paper 1, Q4(b)**: Minimum insertions for palindrome on `"Ab3bd"` $\to$ answer = 2 (`"dAb3bAd"`).

---

## 3.3 Longest Arithmetic Progression Subsequence (LAPS)
* 💡 **Hinglish "Asli Baat"**: Array me aisa subsequence dhoondhna jisme adjacent elements ka difference same ho ($A[k], A[i], A[j]$ where $A[k] + A[j] == 2A[i]$). Array ko sort karo, 2D table banao jahan state $DP[i][j]$ represent karta hai "LAPS ending at index $i$ and $j$".
* 🎯 **Standard English Definition**: Finds the longest sequence of indices $i_1 < \dots < i_k$ such that $A[i_{m+1}] - A[i_m] = d$. Solved in $O(n^2)$ time by sorting $A$ and using 2-pointer scan on the middle element $i$.
* 🛠️ **Algorithm Design & Case-Study Decoder Table**:
  | Scenario / Real-World Trigger | Secret Paradigm | State / Variable Notation | Mathematical Invariant | 4-Section Answer Blueprint |
  | :--- | :--- | :--- | :--- | :--- |
  | Detecting periodic sensor signals or regular spaced patterns in unsorted logs | 2D Dynamic Programming with 2 Pointers | $L[i][j]$ = length of AP ending with $A[i]$ and $A[j]$ ($i < j$) | $A[k] + A[j] == 2A[i]$ implies $A[k], A[i], A[j]$ form AP of step $d = A[j] - A[i]$ | 1. State paradigm: 2D DP on sorted array<br>2. Recurrence: $L[i][j] = L[k][i] + 1$ when $A[k]+A[j]==2A[i]$<br>3. Pseudocode: Fix middle $i$, two pointers $k$ and $j$<br>4. Time: $O(n^2)$, Space: $O(n^2)$ |
* 📚 **Targeted Practice**:
  * **Actual Paper 2, Q3(b)(i)**: Full 7-mark question on Longest Arithmetic Progression Subsequence.

---

## 3.4 Poisson Binomial DP: Probability of Exactly $k$ Heads
* 💡 **Hinglish "Asli Baat"**: $n$ coins hain, sabka chhaap (heads) aane ka chance alag-alag hai ($p_1, p_2 \dots p_n$). Exactly $k$ heads aane ki probability nikaalne ke liye DP table banao: $i$-th coin ya toh heads aayega (with prob $p_i$, contributing to $j-1$ prior heads) ya tails aayega (with prob $1-p_i$, contributing to $j$ prior heads)!
* 🎯 **Standard English Definition**: Computes the probability of exactly $k$ successes out of $n$ independent, non-identically distributed Bernoulli trials with probabilities $p_1, \dots, p_n$ in $O(n \cdot k)$ time and $O(k)$ space.
* 📐 **Core Recurrence**:
  $$DP[i][j] = DP[i-1][j-1] \cdot p_i + DP[i-1][j] \cdot (1 - p_i)$$
  $$\text{Base Cases: } DP[0][0] = 1.0, \quad DP[0][j] = 0.0 \text{ for } j > 0$$
* 🛠️ **Algorithm Design & Case-Study Decoder Table**:
  | Scenario / Real-World Trigger | Secret Paradigm | State / Variable Notation | Mathematical Invariant | 4-Section Answer Blueprint |
  | :--- | :--- | :--- | :--- | :--- |
  | System reliability: $n$ redundant components with failure rates, compute probability exactly $k$ survive | Forward Probability DP | $DP[i][j]$ = prob of $j$ heads among first $i$ coins | Law of Total Probability on $i$-th independent coin toss | 1. Name paradigm: Forward Probability DP<br>2. State recurrence & base cases<br>3. Pseudocode with 1D rolling array optimization<br>4. Time: $O(nk)$, Space: $O(k)$ |
* 📚 **Targeted Practice**:
  * **Actual Paper 2, Q3(b)(ii)**: Full 7-mark question with pseudocode and complexity.

---

## 3.5 Bounded-Length Directed Paths & Floyd-Warshall
* 💡 **Hinglish "Asli Baat"**: Directed graph me $s$ se $t$ tak aise raaste gin-na jinki length at most $k$ ho: $DP[m][v]$ banao jo bataye "length $m$ ke kitne paths vertex $v$ par aate hain". $v$ par aane wale har incoming neighbor $u$ ke path count ko sum karte jao!
* 🎯 **Standard English Definition**: Counts paths from $s$ to $t$ using at most $k$ edges. $P[m][v] = \sum_{(u, v) \in E} P[m-1][u]$. Total paths = $\sum_{m=1}^k P[m][t]$ in $O(k(V+E))$ time.
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 25, Section 25.1**: Dynamic programming formulation for shortest paths of length $\le m$.
  * **Actual Paper 2, Q6(b)**: Count distinct directed paths of length at most $k$ in $O(k(|V|+|E|))$.

---

# ===================================================================
# UNIT IV: GREEDY STRATEGY & BACKTRACKING
# ===================================================================

## 4.1 Huffman Coding & Optimal Prefix Trees
* 💡 **Hinglish "Asli Baat"**: Sabse kam frequency wale do symbols ko utha kar jodo, aur unke sum ko wapas priority queue me daal do. Aisa tab tak karo jab tak single tree na ban jaye. Left branch ko `0` aur right branch ko `1` do. Jo symbol jitna zyada repeat hota hai, uska binary code utna chhota banta hai!
* 🎯 **Standard English Definition**: A greedy algorithm that constructs an optimal prefix-free binary tree with minimum weighted external path length $\sum f_i \cdot d_i$ in $O(n \log n)$ time using a min-heap.
* 📐 **The Exact VIT Pune ESE Verbatim Problem**:
  * Alphabet: $\{a:5, b:7, c:8, d:16, e:18, f:22, g:24\}$ | Total frequency = 100.
  * **Merge Steps**:
    1. Merge $(a:5, b:7) \to N_1:12$
    2. Merge $(c:8, N_1:12) \to N_2:20$
    3. Merge $(d:16, e:18) \to N_3:34$
    4. Merge $(f:22, g:24) \to N_4:46$
    5. Merge $(N_2:20, N_3:34) \to N_5:54$
    6. Merge $(N_4:46, N_5:54) \to \text{Root}:100$
  * **Resulting Codewords**:
    * $f (22): \mathbf{01}$ (2 bits)
    * $g (24): \mathbf{10}$ (2 bits)
    * $c (8): \mathbf{000}$ (3 bits)
    * $d (16): \mathbf{110}$ (3 bits)
    * $e (18): \mathbf{111}$ (3 bits)
    * $a (5): \mathbf{0010}$ (4 bits)
    * $b (7): \mathbf{0011}$ (4 bits)
  * **Average Codeword Length**:
    $$L_{avg} = \frac{22(2) + 24(2) + 8(3) + 16(3) + 18(3) + 5(4) + 7(4)}{100} = \frac{266}{100} = \mathbf{2.66\text{ bits/char}}$$
  * **Total Size for 100,000 characters**:
    $$\text{Size} = 100,000 \times 2.66 = \mathbf{266,000\text{ bits}} = \mathbf{33,250\text{ bytes}}$$
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 16, Section 16.3**: Elements of the Huffman code, correctness proof (Lemmas 16.2 & 16.3).
  * **Appeared in BOTH Actual Paper 1 Q3(a)(ii) and Actual Paper 2 Q3(a)**!

---

## 4.2 Backtracking: 4-Queens & Bounding Functions
* 💡 **Hinglish "Asli Baat"**: 4-Queens me solution vector $X[1..4]$ hota hai jahan $X[k]$ batata hai ki $k$-th row ki rani kaunse column me hai. Bounding condition: Do rani same column me nahi ho sakti ($X[k] \ne X[i]$) aur same diagonal me nahi ho sakti ($|X[k] - X[i]| \ne |k - i|$). Agar koi column valid na mile, toh pichli row par backtrack karo!
* 🎯 **Standard English Definition**: Backtracking systematically explores a state-space tree depth-first. At each level $k$, it prunes dead ends using a bounding function without exploring futile subtrees.
* 📐 **State Representation & Bounding Invariant**:
  * Solution tuple: $(x_1, x_2, \dots, x_n)$ where $x_k \in \{1..n\}$.
  * **Bounding Check**:
    $$\text{Place}(k, i) = \text{True} \iff \forall j \in \{1..k-1\}, \quad x_j \ne i \text{ and } |x_j - i| \ne |j - k|$$
  * **Solutions for 4-Queens**:
    * Solution 1: $(2, 4, 1, 3)$
    * Solution 2: $(3, 1, 4, 2)$
* 📚 **Targeted Practice**:
  * **Actual Paper 2, Q2(a)**: 5-mark question: Backtracking strategy, 4-queens example, bounding condition, and explicit backtrack step.

---

## 4.3 Greedy / DP on Trees (Maximum Independent Set / Blue Vertices)
* 💡 **Hinglish "Asli Baat"**: Tree par koi cycle nahi hoti! Isliye agar examiner puche: "Tree ke maximum vertices ko blue color karo taaki koi do blue adjacent na hon", toh yeh graph coloring ya independent set hai. Tree me bas leaves se shuru karo: har leaf ko blue color karo, uske parent ko disqualify karo, aur upar badhte jao in $O(n)$!
* 🎯 **Standard English Definition**: Finding Maximum Weight Independent Set on a tree. Unlike general graphs (NP-Complete), on trees this is solved in deterministic $O(n)$ using post-order tree dynamic programming or bottom-up greedy leaf pruning.
* 📐 **Tree DP Formulation**:
  For each vertex $u$, define:
  * $DP[u, 0]$ = max blue vertices in subtree rooted at $u$ when $u$ is **NOT** colored blue.
  * $DP[u, 1]$ = max blue vertices in subtree rooted at $u$ when $u$ **IS** colored blue.
  $$DP[u, 0] = \sum_{v \in children(u)} \max(DP[v, 0], DP[v, 1])$$
  $$DP[u, 1] = 1 + \sum_{v \in children(u)} DP[v, 0]$$
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 15, Problem 15-5 (Viterbi algorithm) & Problem 15-7 (Maximum independent set in a tree)**.
  * **Actual Paper 1, Q1(b)**: Color maximum vertices blue with no two adjacent blue in a tree.

---

# ===================================================================
# UNIT V: NETWORK FLOW OPTIMIZATION
# ===================================================================

## 5.1 Flow Network Axioms, Residuals & Edmonds-Karp
* 💡 **Hinglish "Asli Baat"**: Network flow me 3 rules hote hain: (1) Pipe ki capacity se zyada paani nahi ja sakta, (2) जितना paani aaya utna hi bahar nikalna chahiye (conservation), (3) Forward edge me jagah bachi hai ($c - f$) aur backward edge me bheja hua flow wapas lene ki capability ($f$) hoti hai. Edmonds-Karp BFS se shortest path dhoondhta hai taaki infinite loop na bane!
* 🎯 **Standard English Definition**: A flow network is a directed graph where edges have capacities $c(u,v) \ge 0$. Edmonds-Karp implements Ford-Fulkerson using BFS to find augmenting paths with the fewest edges, running in $O(V E^2)$ time.
* 📐 **The 3 Fundamental Flow Axioms**:
  1. **Capacity Constraint**: $0 \le f(u, v) \le c(u, v) \quad \forall (u, v) \in E$.
  2. **Skew Symmetry**: $f(u, v) = -f(v, u)$.
  3. **Flow Conservation**: $\sum_{v \in V} f(u, v) = 0 \quad \forall u \in V \setminus \{s, t\}$.
* 📐 **Edmonds-Karp Complexity Derivation**:
  In each augmentation, at least one edge becomes critical. Each of the $|E|$ edges can become critical at most $|V|/2$ times because shortest path distances $\delta_f(s, u)$ monotonically increase. Total augmentations $\le O(VE)$. With BFS taking $O(E)$, total time = $\mathbf{O(V E^2)}$.
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 26, Section 26.2**: The Ford-Fulkerson method (Lemma 26.2, Theorem 26.8).

---

## 5.2 Max-Flow Min-Cut Theorem & Bipartite Matching
* 💡 **Hinglish "Asli Baat"**: 
  * *Min-Cut*: Graph me source $s$ aur sink $t$ ko alag karne wali sabse sasti diwaar (cut) ki capacity hamesha maximum flow ke barabar hoti hai!
  * *Bipartite Matching*: Boys aur Girls ka match karana hai toh ek Super-Source $s$ banao jo sabhi boys ko 1 capacity se connect kare, aur sabhi girls ko Super-Sink $t$ se 1 capacity me jodo. Max Flow = Maximum Matching!
* 📐 **Max-Flow Min-Cut Statement**:
  The following three statements are equivalent:
  1. $f$ is a maximum flow in $G$.
  2. The residual network $G_f$ contains no augmenting paths.
  3. $|f| = c(S, T)$ for some cut $(S, T)$ of $G$ where $S = \{v \in V \mid \text{path from } s \text{ to } v \text{ in } $G_f\}$.
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 26, Section 26.3**: Maximum bipartite matching (Figure 26.8).

---

# ===================================================================
# UNIT VI: NP-COMPLETENESS, APPROXIMATION & RANDOMIZATION
# ===================================================================

## 6.1 Complexity Classes & The Prover-Verifier Definition
* 💡 **Hinglish "Asli Baat"**: 
  * **P**: Jo problem polynomial time me solve ho jaye.
  * **NP**: Jiska solution agar koi de de (certificate), toh hum polynomial time me verify kar lein ki solution sahi hai ya galat.
  * **NP-Hard**: Wo problem jo NP ki sabhi problems se kam se kam utni hi kathin hai.
  * **NP-Complete**: Jo NP me bhi ho aur NP-Hard bhi ho!
* 🎯 **Standard English Definition**:
  * **NP (Verifier Definition)**: Language $L \in \text{NP} \iff \exists$ polynomial-time algorithm $V(x, w)$ and polynomial $p$ such that $x \in L \iff \exists w$ with $|w| \le p(|x|)$ where $V(x, w) = \text{ACCEPT}$.
  * **Polynomial Reduction ($A \le_p B$)**: A polynomial-time function $f$ mapping instances of $A$ to $B$ such that $x \in A \iff f(x) \in B$.
* 📚 **Targeted Practice**:
  * **Actual Paper 2, Q2(b)(i)**: Full 5-mark question on Prover-Verifier definition, NP-Hard, NP-Complete, and concrete YES/NO instances for 3-SAT and VERTEX COVER.

---

## 6.2 3-SAT to CLIQUE & VERTEX COVER Reduction
* 💡 **Hinglish "Asli Baat"**: 3-SAT se CLIQUE banana bohot easy hai: har clause ke 3 literals ke liye 3 vertices banao. Do alag-alag clauses ke vertices ke beech edge tabhi banao agar wo ek doosre ke contradictory na hon ($x$ aur $\neg x$ na hon). 3-SAT true hoga if and only if graph me $k$ size ka CLIQUE milega ($k$ = number of clauses)!
* 📐 **Reduction Construction**:
  Given 3-CNF formula $\phi = C_1 \wedge C_2 \dots \wedge C_k$ with $k$ clauses:
  * For each clause $C_r = (l_1^r \vee l_2^r \vee l_3^r)$, create a cluster of 3 vertices $v_1^r, v_2^r, v_3^r$.
  * Put an edge between $v_i^r$ and $v_j^s$ if and only if:
    1. $r \ne s$ (they belong to distinct clauses).
    2. $l_i^r \ne \neg l_j^s$ (they are not contradictory literals).
  * **Theorem**: $\phi$ is satisfiable $\iff G$ has a clique of size $k$.
  * **Vertex Cover Connection**: $G$ has a clique of size $k \iff$ Complement graph $\overline{G}$ has a vertex cover of size $|V| - k$.
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 34, Section 34.5**: NP-completeness proofs (Reduction 3-CNF-SAT to CLIQUE, Theorem 34.11).
  * **Actual Paper 1, Q5(a)**: VERTEX COVER to CLIQUE reduction proof.

---

## 6.3 2-Approximation Algorithms: Vertex Cover & Metric TSP
* 💡 **Hinglish "Asli Baat"**: 
  * *Vertex Cover*: Graph me Maximal Matching banao (edges chunte jao jinka koi vertex shared na ho). Matching ke dono endpoints ko answer me daal do. Answer guaranteed $\le 2 \times \text{OPT}$ hoga!
  * *Metric TSP*: Minimum Spanning Tree (MST) banao. Har edge ko double karke Euler tour chalao. Duplicate vertices ko triangle inequality se shortcut karo. Total cost $\le 2 \times \text{weight(MST)} \le 2 \times \text{OPT}$!
* 📐 **Vertex Cover 2-Approximation Proof**:
  Let $M$ be a maximal matching. Every vertex cover must pick at least one endpoint from each disjoint edge in $M \implies \text{OPT} \ge |M|$.
  Our algorithm picks both endpoints of every edge in $M \implies |C| = 2|M|$.
  Therefore: $|C| = 2|M| \le 2 \cdot \text{OPT}$. Ratio = **2**.
* 📚 **Targeted CLRS Problems**:
  * **CLRS Ch. 35, Section 35.1 (Vertex Cover)** & **Section 35.2 (TSP with Triangle Inequality)**.

---

## 6.4 Randomized Algorithms: Monte Carlo vs Las Vegas (Popular Element $\ge n/3$)
* 💡 **Hinglish "Asli Baat"**: 
  * **Monte Carlo**: Time fix hota hai par galat answer ka chhota sa chance rehta hai. $m$ random elements uthao, check karo. Agar error chance $\le 3^{-100}$ chahiye, toh $m = 271$ samples lo.
  * **Las Vegas**: Answer hamesha 100% sahi hoga, par time random ho sakta hai. Random element uthao aur test karo; agar popular hai toh return, varna retry. Average sirf 3 attempts lagenge ($E[T] \le 3$)!
* 📐 **Core Error Bound Calculations**:
  * For element frequency $\ge n/3$, failure probability in 1 trial $\le (1 - 1/3) = 2/3$.
  * Failure in $m$ independent trials: $P(\text{fail}) \le (2/3)^m$.
  * To achieve $P(\text{fail}) \le 3^{-100}$:
    $$m \ln(2/3) \le -100 \ln(3) \implies m \ge \frac{100 \ln 3}{\ln(1.5)} = \frac{109.86}{0.4055} \approx \mathbf{271\text{ trials}}$$
  * **Las Vegas Expected Time**:
    $$E[\text{trials}] = \frac{1}{p} \le \frac{1}{1/3} = 3 \implies E[\text{Time}] = 3 \times O(n) = \mathbf{\Theta(n)}$$
* 📚 **Targeted Practice**:
  * **Actual Paper 2, Q5**: Full 8-mark question on Monte Carlo vs Las Vegas for popular element $\ge n/3$.

---

## 6.5 Discrete Geometry & Planar Point Bisection
* 💡 **Hinglish "Asli Baat"**: 
  * *Necklace Bisection*: $2n$ black aur $2n$ white beads ka necklace hai. 2 cuts laga kar exact $n$ black aur $n$ white beads alag kiye ja sakte hain via Discrete Intermediate Value Theorem (difference function $D(i)$ signs flip karta hai).
  * *Red-Blue Points*: Plane me $n$ red aur $n$ blue points hain. Kisi ek extreme red point se line ghuma kar radial sweep karo taaki dono taraf equal red aur blue points bat jayein. Phir divide & conquer se bina intersect kiye match kar do!
* 📚 **Targeted Practice**:
  * **Actual Paper 1, Q4(a)**: 2-cut bracelet necklace bisection with IVT proof.
  * **Actual Paper 2, Q4**: Ham-Sandwich planar red-blue point bisection and non-intersecting matching.

---

## 🏁 Summary Checklist Before You Enter the Exam Hall

1. **Catalan Numbers**: Write down $C_6 = 132$ and $C_7 = 429$ the moment you see parenthesizations or full binary trees.
2. **Huffman Question**: If $\{a:5, b:7, c:8, d:16, e:18, f:22, g:24\}$ appears, total size is **$266,000\text{ bits}$** and average length is **$2.66$**.
3. **Modular Exponentiation**: Remember $4444 \equiv -2 \pmod 9$ and $(-2)^3 \equiv 1 \pmod 9 \implies \text{Digital Root} = \mathbf{7}$.
4. **Any Case-Study Question**: Use the 4-Section Template: (1) Paradigm & State, (2) Recurrence & Base Case, (3) Pseudocode with simple arrays, (4) Inductive Correctness & $O$-complexity.
