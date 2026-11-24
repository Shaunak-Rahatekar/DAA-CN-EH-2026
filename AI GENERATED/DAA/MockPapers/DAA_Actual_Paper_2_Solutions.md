# Solutions & Marking Scheme: VIT Pune End-Semester Examination 2
## Course: Design and Analysis of Algorithms (CS 3205 / CS 3212)

**Exam**: End-Semester Examination (ESE) | **Year**: TY | **Branch**: Computer Engineering  
**Subject**: Design and Analysis of Algorithms | **Code**: CS 3205  
**Max. Marks**: 60 | **Duration**: 2 Hours  

---

## QUESTION 1 [10 Marks]

### Q.1. (a) Asymptotic Bounds on Recurrences [4 Marks]

For the following two recurrences give best possible asymptotic upper bound on $T(n)$ as a function of $n$. Express your answer using Big Oh asymptotic notation.

#### 1. $T(n) = T(\sqrt{n}) + 1$ [2 Marks]
* **Solution via Variable Substitution**:
  Let $n = 2^m$, which implies $m = \log_2 n$.
  Then $\sqrt{n} = n^{1/2} = (2^m)^{1/2} = 2^{m/2}$.
  Define a new recurrence $S(m) = T(2^m) = T(n)$.
  Substituting into the recurrence:
  $$S(m) = S(m/2) + 1$$
  This is the classic binary search recurrence.
  Applying the Master Theorem ($a = 1, b = 2, f(m) = 1$):
  $$m^{\log_b a} = m^{\log_2 1} = m^0 = 1 = \Theta(f(m)) \implies S(m) = \Theta(\log_2 m)$$
  Now substitute back $m = \log_2 n$:
  $$T(n) = S(\log_2 n) = \Theta(\log_2(\log_2 n)) = \mathbf{O(\log \log n)}$$
* **Result**: **$T(n) = O(\log \log n)$**

---

#### 2. $T(n) = 3 T(n/2) + n$ [2 Marks]
* **Solution via Master Theorem**:
  Standard form: $T(n) = a T(n/b) + f(n)$ where $a = 3, b = 2, f(n) = n$.
  Compute the watershed critical exponent:
  $$n^{\log_b a} = n^{\log_2 3} \approx n^{1.585}$$
  Compare $f(n) = n = n^1$ with $n^{\log_2 3}$:
  Since $1 < \log_2 3 \approx 1.585$, we have:
  $$f(n) = O(n^{\log_2 3 - \epsilon}) \quad \text{for } \epsilon = \log_2 3 - 1 \approx 0.585 > 0$$
  This satisfies **Case 1 of the Master Theorem**.
  Therefore:
  $$T(n) = \Theta(n^{\log_b a}) = \mathbf{\Theta(n^{\log_2 3})} = \mathbf{O(n^{\log_2 3})} \approx \mathbf{O(n^{1.585})}$$
* **Result**: **$T(n) = O(n^{\log_2 3})$**

---

### Q.1. (b) Matrix Chain Multiplication Parenthesization & Catalan Numbers [6 Marks]

#### (1) Recurrence Relation for $f(n)$ [2 Marks]
* Let $f(n)$ denote the number of distinct ways to parenthesize a chain of $n$ matrices $A_1 A_2 \dots A_n$.
* To compute the product of $n$ matrices, the outermost multiplication splits the chain into two sub-chains:
  $$(A_1 A_2 \dots A_k) \times (A_{k+1} \dots A_n) \quad \text{for } 1 \le k \le n - 1$$
* The left sub-chain contains $k$ matrices, which can be parenthesized in $f(k)$ ways.
* The right sub-chain contains $n - k$ matrices, which can be parenthesized in $f(n - k)$ ways.
* By the multiplication and addition principles of combinatorics:
  $$f(n) = \sum_{k=1}^{n-1} f(k) \cdot f(n - k) \quad \text{for } n \ge 2, \quad \text{with base case } f(1) = 1$$
* **Verification**:
  - $f(2) = f(1)f(1) = 1 \times 1 = 1$
  - $f(3) = f(1)f(2) + f(2)f(1) = 1(1) + 1(1) = \mathbf{2}$ (Matches example!)
  - $f(4) = f(1)f(3) + f(2)f(2) + f(3)f(1) = 1(2) + 1(1) + 2(1) = \mathbf{5}$ (Matches example!)

#### (2) Values of $f(7)$ and $f(8)$ [2 Marks]
Notice that $f(n) = C_{n-1}$, the $(n-1)$-th Catalan number, where $C_m = \frac{1}{m+1}\binom{2m}{m}$.
- $f(5) = C_4 = 14$
- $f(6) = C_5 = 42$
- **$f(7) = C_6$**:
  $$f(7) = \frac{1}{7}\binom{12}{6} = \frac{1}{7} \cdot \frac{12 \times 11 \times 10 \times 9 \times 8 \times 7}{6 \times 5 \times 4 \times 3 \times 2 \times 1} = \frac{1}{7} \cdot 924 = \mathbf{132}$$
- **$f(8) = C_7$**:
  $$f(8) = \frac{1}{8}\binom{14}{7} = \frac{1}{8} \cdot \frac{14 \times 13 \times 12 \times 11 \times 10 \times 9 \times 8}{7 \times 6 \times 5 \times 4 \times 3 \times 2 \times 1} = \frac{1}{8} \cdot 3432 = \mathbf{429}$$

#### (3) Proof that $f(n) = \Omega(2^n)$ [2 Marks]
* **Proof 1: Induction on Sub-Sum Bounding**:
  For $n \ge 3$, retain only the first and last terms in the summation for $f(n)$:
  $$f(n) = \sum_{k=1}^{n-1} f(k) f(n - k) \ge f(1) f(n - 1) + f(n - 1) f(1) = 1 \cdot f(n - 1) + f(n - 1) \cdot 1 = 2 f(n - 1)$$
  By repeated unfolding:
  $$f(n) \ge 2 f(n - 1) \ge 2^2 f(n - 2) \ge \dots \ge 2^{n - 2} f(2) = 2^{n - 2} \cdot 1 = \frac{1}{4} \cdot 2^n$$
  Choosing constant $c = \frac{1}{4}$ and $n_0 = 2$, we have $f(n) \ge c \cdot 2^n$ for all $n \ge n_0$.
  Therefore, by definition of Big-Omega:
  $$\mathbf{f(n) = \Omega(2^n)} \quad \blacksquare$$

---

## QUESTION 2 [10 Marks]

### Q.2. (a) Backtracking Strategy & $N$-Queens Problem [5 Marks]

#### 1. Concept of Backtracking [1.5 Marks]
* **Backtracking** is a refined depth-first search strategy for solving Constraint Satisfaction Problems (CSPs).
* It builds candidate solutions incrementally. At each level of the state-space tree, a bounding function evaluates whether the partial candidate satisfies all constraints.
* If a constraint is violated, the branch is **pruned**, the tentative choice is **undone (the backtracking step)**, and the algorithm backtracks to the parent state to explore alternative siblings.

#### 2. $N$-Queens Problem Formulation [2 Marks]
* **Goal**: Place $N$ chess queens on an $N \times N$ chessboard such that no two queens attack each other (no two in the same row, column, or diagonal).
* **State Representation**: A 1D array `X[1..N]`, where `X[i]` represents the column index of the queen placed in row `i`. This automatically ensures no two queens share a row.
* **Safety / Bounding Function `isSafe(k, col)`**:
  A queen placed at `(k, col)` is safe if for all previously placed queens in rows $i \in \{1, \dots, k-1\}$:
  1. `X[i] != col` (Different columns)
  2. `abs(X[i] - col) != abs(i - k)` (Different diagonals: slope $\ne \pm 1$)

#### 3. Algorithm & Backtracking Step [1.5 Marks]
```text
Algorithm NQueens(k, N, X):
    For col from 1 to N:
        If isSafe(k, col, X):
            X[k] = col  // Place queen in row k, column col

            If k == N:
                PrintSolution(X)  // Found complete solution
            Else:
                NQueens(k + 1, N, X)  // Recurse to next row

            // BACKTRACKING STEP:
            // X[k] is implicitly undone when the loop advances to col + 1
```
* **4-Queens Example Trace**:
  - Row 1: place `X[1] = 1`
  - Row 2: `col=1, 2` attack. Place `X[2] = 3`
  - Row 3: `col=1, 2, 3, 4` all attacked $\implies$ **Dead end! Backtrack to Row 2.**
  - Row 2: Try `X[2] = 4`.
  - Row 3: `col=2` is safe $\implies$ `X[3] = 2`.
  - Row 4: `col=1, 2, 3, 4` all attacked $\implies$ **Backtrack to Row 1!**
  - Row 1: Try `X[1] = 2` $\implies$ leads to valid solution `[2, 4, 1, 3]`.

---

### Q.2. (b) Attempt Any ONE [5 Marks]

#### OPTION (b)(i): Complexity Classes & NP-Completeness [5 Marks]

* **1. Prover-Verifier Definition of Class NP [1.5 Marks]**:
  A language $L \subseteq \{0, 1\}^*$ belongs to **NP** (Nondeterministic Polynomial Time) if there exists a deterministic polynomial-time verifier algorithm $V(x, w)$ and a polynomial $p$ such that:
  $$x \in L \iff \exists w \in \{0, 1\}^* \text{ with } |w| \le p(|x|) \text{ such that } V(x, w) = \text{ACCEPT}$$
  The string $w$ is the **certificate (proof)**. The prover provides the certificate; the verifier $V$ efficiently verifies it in deterministic polynomial time.

* **2. Definitions of NP-Hard and NP-Complete [1.5 Marks]**:
  - **NP-Hard**: A problem $B$ is NP-hard if for every problem $A \in \text{NP}$, $A$ polynomial-time reduces to $B$ ($A \le_P B$). It is at least as hard as any problem in NP.
  - **NP-Complete**: A problem $B$ is NP-complete if:
    1. $B \in \text{NP}$ (its solutions can be verified in polynomial time), AND
    2. $B$ is NP-hard ($A \le_P B$ for all $A \in \text{NP}$).

* **3. Two Concrete Examples of NP-Complete Problems [2 Marks]**:
  1. **3-SAT (3-Conjunctive Normal Form Satisfiability)**:
     - *Input*: A boolean formula $\Phi = \bigwedge_{i=1}^m (l_{i,1} \vee l_{i,2} \vee l_{i,3})$ with $m$ clauses over variables $x_1, \dots, x_n$.
     - *YES Instance*: There exists a truth assignment $\tau$ such that $\Phi(\tau) = \text{True}$.
     - *NO Instance*: No truth assignment satisfies $\Phi$.
  2. **VERTEX COVER**:
     - *Input*: An undirected graph $G = (V, E)$ and an integer $k \ge 1$.
     - *YES Instance*: There exists a subset $V' \subseteq V$ with $|V'| \le k$ such that every edge $(u, v) \in E$ has at least one endpoint in $V'$.
     - *NO Instance*: No vertex cover of size $\le k$ exists.

---

#### OPTION (b)(ii): Digital Root of $4444^{4444}$ [5 Marks]

* **1. Mathematical Principle of the Digital Root**:
  In base 10, powers of 10 satisfy $10 \equiv 1 \pmod 9$, so $10^k \equiv 1 \pmod 9$.
  Any natural number $n = \sum a_k 10^k$ satisfies:
  $$n \equiv \sum a_k \pmod 9$$
  Taking the sum of digits preserves the remainder modulo 9. Therefore, the digital root satisfies:
  $$\text{root}(n) \equiv n \pmod 9$$
  Specifically, for any $n > 0$:
  $$\text{root}(n) = \begin{cases} n \bmod 9 & \text{if } n \not\equiv 0 \pmod 9 \\ 9 & \text{if } n \equiv 0 \pmod 9 \end{cases}$$

* **2. Step-by-Step Reduction of $4444^{4444} \pmod 9$**:
  - **Step 2.1: Reduce the Base Modulo 9**:
    Sum of digits of $4444 = 4 + 4 + 4 + 4 = 16$.
    $$16 = 1 \times 9 + 7 \implies 4444 \equiv 7 \equiv -2 \pmod 9$$
  - **Step 2.2: Evaluate Exponent Cycles**:
    $$4444^{4444} \equiv (-2)^{4444} \pmod 9$$
    Compute small powers of $-2$ modulo 9:
    - $(-2)^1 \equiv -2 \equiv 7 \pmod 9$
    - $(-2)^2 \equiv 4 \pmod 9$
    - $(-2)^3 \equiv -8 \equiv 1 \pmod 9$
    Since $(-2)^3 \equiv 1 \pmod 9$, the multiplicative order of $-2$ modulo 9 is **3**.
  - **Step 2.3: Reduce the Exponent Modulo 3**:
    Divide the exponent $4444$ by 3:
    $$4444 = 3 \times 1481 + 1 \implies 4444 \equiv 1 \pmod 3$$
  - **Step 2.4: Final Calculation**:
    $$4444^{4444} \equiv (-2)^{4444} = \big((-2)^3\big)^{1481} \cdot (-2)^1 \equiv (1)^{1481} \cdot (-2) \equiv -2 \equiv \mathbf{7} \pmod 9$$

* **Conclusion**:
  Since $4444^{4444} \equiv 7 \pmod 9$ and $7 \ne 0$:
  $$\mathbf{\text{root}(4444^{4444}) = 7}$$

---

## QUESTION 3 [12 Marks]

### Q.3. (a) Huffman Coding [5 Marks]
Alphabet $\{a, b, c, d, e, f, g\}$ with frequencies $5, 7, 8, 16, 18, 22, 24$. Document length: $100,000$ characters.

* **1. Huffman Tree Construction**:
  1. Priority Queue: $\{a: 5, b: 7, c: 8, d: 16, e: 18, f: 22, g: 24\}$
  2. Combine $a(5)$ and $b(7) \to N_1(12)$. Queue: $\{c: 8, N_1: 12, d: 16, e: 18, f: 22, g: 24\}$
  3. Combine $c(8)$ and $N_1(12) \to N_2(20)$. Queue: $\{d: 16, e: 18, N_2: 20, f: 22, g: 24\}$
  4. Combine $d(16)$ and $e(18) \to N_3(34)$. Queue: $\{N_2: 20, f: 22, g: 24, N_3: 34\}$
  5. Combine $N_2(20)$ and $f(22) \to N_4(42)$. Queue: $\{g: 24, N_3: 34, N_4: 42\}$
  6. Combine $g(24)$ and $N_3(34) \to N_5(58)$. Queue: $\{N_4: 42, N_5: 58\}$
  7. Combine $N_4(42)$ and $N_5(58) \to \text{Root}(100)$.

* **2. Optimal Prefix Codes (Left = 0, Right = 1)**:
  - $\mathbf{f = 01}$ (Length 2)
  - $\mathbf{g = 10}$ (Length 2)
  - $\mathbf{c = 000}$ (Length 3)
  - $\mathbf{d = 110}$ (Length 3)
  - $\mathbf{e = 111}$ (Length 3)
  - $\mathbf{a = 0010}$ (Length 4)
  - $\mathbf{b = 0011}$ (Length 4)

* **3. Encoded Text Size**:
  $$\text{Total Bits per 100 chars} = 5(4) + 7(4) + 8(3) + 16(3) + 18(3) + 22(2) + 24(2) = 266 \text{ bits}$$
  $$\text{Average Bit Length} = \frac{266}{100} = \mathbf{2.66 \text{ bits/char}}$$
  $$\text{Total Encoded Size for 100,000 characters} = 100,000 \times 2.66 = \mathbf{266,000 \text{ bits}} \quad (33,250 \text{ bytes})$$

---

### Q.3. (b) Attempt Any ONE [7 Marks]

#### OPTION (b)(i): Longest Arithmetic Progression Subsequence [7 Marks]

* **Algorithm Design (Dynamic Programming)**:
  1. Sort array $A$ in ascending order in $O(n \log n)$ time. Any arithmetic progression will appear in sorted order.
  2. Define DP table: $L[i][j]$ = length of the longest arithmetic progression ending with elements $A[i]$ and $A[j]$ ($i < j$).
  3. Base case: Any pair of elements forms an AP of length 2: $L[i][j] = 2$.
  4. For a fixed middle element $A[i]$, we look for $A[k]$ and $A[j]$ such that $A[k] + A[j] = 2 A[i]$ (meaning $A[i] - A[k] = A[j] - A[i]$).
  5. We can evaluate all pairs using a two-pointer scan:
     - If $A[k] + A[j] == 2 A[i]$: $L[i][j] = L[k][i] + 1$. Decrement $k$, increment $j$.
     - If $A[k] + A[j] < 2 A[i]$: $j$ is too small, increment $j$.
     - If $A[k] + A[j] > 2 A[i]$: $k$ is too large, decrement $k$.

* **Pseudocode**:
  ```text
  Algorithm LongestArithmeticSubsequence(A, n):
      Sort(A)
      If n <= 2: return n

      Initialize 2D array L[1..n][1..n] to 2
      max_len = 2

      For i from 2 to n - 1:
          k = i - 1
          j = i + 1
          While (k >= 1 and j <= n):
              If A[k] + A[j] == 2 * A[i]:
                  L[i][j] = L[k][i] + 1
                  max_len = max(max_len, L[i][j])
                  k = k - 1
                  j = j + 1
              Else if A[k] + A[j] < 2 * A[i]:
                  j = j + 1
              Else:
                  k = k - 1

      return max_len
  ```

* **Complexity**:
  - Sorting takes $O(n \log n)$.
  - The outer loop runs $n - 2$ times. The inner two-pointer search takes $O(n)$ steps because $k$ only decrements and $j$ only increments.
  - **Time Complexity**: $\mathbf{O(n^2)}$.
  - **Space Complexity**: $\mathbf{O(n^2)}$ for table $L$.

---

#### OPTION (b)(ii): Probability of Exactly $k$ Heads from $n$ Biased Coins [7 Marks]

* **Algorithm Design (Poisson Binomial Dynamic Programming)**:
  Let $DP[i][j]$ denote the probability of obtaining exactly $j$ heads when tossing the first $i$ independent coins ($0 \le i \le n, 0 \le j \le k$).
  When tossing the $i$-th coin:
  - It turns up Heads with probability $p_i$, contributing 1 head.
  - It turns up Tails with probability $1 - p_i$, contributing 0 heads.

* **Recurrence Relation**:
  $$DP[i][j] = DP[i - 1][j - 1] \cdot p_i + DP[i - 1][j] \cdot (1 - p_i)$$

* **Base Cases**:
  - $DP[0][0] = 1.0$ (0 coins, 0 heads has probability 1).
  - $DP[0][j] = 0.0$ for all $j \ge 1$ (impossible to get $>0$ heads with 0 coins).
  - $DP[i][0] = DP[i-1][0] \cdot (1 - p_i) = \prod_{m=1}^i (1 - p_m)$ (all $i$ coins result in Tails).

* **Pseudocode**:
  ```text
  Algorithm ProbabilityKHeads(n, k, p):
      Initialize DP[0..n][0..k] to 0.0
      DP[0][0] = 1.0

      For i from 1 to n:
          DP[i][0] = DP[i - 1][0] * (1.0 - p[i])
          For j from 1 to min(i, k):
              DP[i][j] = DP[i - 1][j - 1] * p[i] + DP[i - 1][j] * (1.0 - p[i])

      return DP[n][k]
  ```

* **Complexity**:
  - **Time Complexity**: The outer loop runs $n$ times; inner loop runs at most $k$ times. Each arithmetic operation takes $O(1)$. Total Time: $\mathbf{O(n \cdot k)}$.
  - **Space Complexity**: $\mathbf{O(n \cdot k)}$, which can be space-optimized to $\mathbf{O(k)}$ by keeping only the previous row $DP[i-1]$.

---

## QUESTION 4 [8 Marks]

### Ham-Sandwich Non-Crossing Bipartite Point Matching [8 Marks]

Given $n$ Red points and $n$ Blue points in general position (no three collinear).

#### 1. Finding Bisecting Red-Blue Pair [6 Marks]
* **Algorithm (Rotational Sweep & Invariant)**:
  1. Choose an extreme point, say the Red point $R_0$ with minimum $x$-coordinate.
  2. For each other point $p \in (R \cup B) \setminus \{R_0\}$, compute its polar angle with respect to $R_0$: $\theta(p) = \text{atan2}(y_p - y_{R_0}, x_p - x_{R_0})$.
  3. Sort all $2n - 1$ points by polar angle in $O(n \log n)$ time: $p_1, p_2, \dots, p_{2n-1}$.
  4. As a ray rotates counter-clockwise around $R_0$, let $R(i)$ and $B(i)$ be the number of Red and Blue points encountered among $\{p_1, \dots, p_i\}$.
  5. Define the discrepancy: $\Delta(i) = R(i) - B(i)$.
     - Before the sweep begins ($i = 0$): $\Delta(0) = 0$.
     - After all points are swept ($i = 2n - 1$): there are $n - 1$ other Red points and $n$ Blue points, so $\Delta(2n - 1) = (n - 1) - n = -1$.
  6. Each point increases or decreases $\Delta$ by exactly 1:
     $|\Delta(i) - \Delta(i-1)| = 1$.
  7. Since $\Delta(0) = 0$ and $\Delta(2n - 1) = -1$, there must exist an index $i^*$ where a Blue point $B^* = p_{i^*}$ is encountered such that the line through $R_0$ and $B^*$ has:
     $$R(i^* - 1) = B(i^* - 1) = k \quad (\text{for some } 0 \le k < n)$$
  8. The ray $R_0 B^*$ contains exactly $k$ Red and $k$ Blue points on one side, and $(n - 1 - k)$ Red and $(n - 1 - k)$ Blue points on the other side!
  9. **Time Complexity**: Finding the extreme point takes $O(n)$, sorting by angle takes $\mathbf{O(n \log n)}$, and linear scan takes $O(n)$. Total: $\mathbf{O(n \log n)}$.

#### 2. Non-Intersecting Red-Blue Pairing [2 Marks]
* **Divide-and-Conquer Algorithm**:
  ```text
  Algorithm MatchRedBlue(R, B):
      If |R| == 0: return empty
      If |R| == 1: connect the single Red point to the single Blue point; return

      (R_cut, B_cut) = FindBisectingPair(R, B)
      Pair(R_cut, B_cut)

      // Subproblem 1 (one side of line R_cut B_cut)
      MatchRedBlue(R_left, B_left)

      // Subproblem 2 (other side of line R_cut B_cut)
      MatchRedBlue(R_right, B_right)
  ```
* **Correctness**: The segment $(R_{\text{cut}}, B_{\text{cut}})$ lies entirely on the dividing line. All pairs formed in `MatchRedBlue(R_left, B_left)` lie strictly in the open left half-plane, and all pairs in `MatchRedBlue(R_right, B_right)` lie strictly in the open right half-plane. Therefore, no segment from the left half-plane can ever intersect any segment from the right half-plane, nor can they intersect $(R_{\text{cut}}, B_{\text{cut}})$.

---

## QUESTION 5 [8 Marks]

### Popular Element ($\ge n/3$ occurrences) in Array [8 Marks]

#### 1. Monte Carlo Algorithm ($P(\text{Correct}) \ge 1 - 3^{-100}$) [4 Marks]
* **Mathematical Basis**:
  Since the popular element $a^*$ appears at least $n/3$ times, the probability of selecting $a^*$ in a single random draw is:
  $$p \ge \frac{n/3}{n} = \frac{1}{3}$$
  The probability of FAILING to select $a^*$ in a single random draw is at most $1 - \frac{1}{3} = \frac{2}{3}$.
  If we draw $m$ independent uniform random samples from $A$:
  $$P(\text{All } m \text{ samples fail}) \le \left(\frac{2}{3}\right)^m$$
  We require this failure probability to be at most $3^{-100}$:
  $$\left(\frac{2}{3}\right)^m \le 3^{-100} \iff m \ln(1.5) \ge 100 \ln 3 \implies m \ge 100 \cdot \frac{1.0986}{0.4055} \approx 271$$
* **Algorithm**:
  1. Draw $m = 271$ independent uniform random samples from $A$: $S = \{A[r_1], \dots, A[r_{271}]\}$.
  2. Count occurrences of each unique candidate in $S$ across the entire array $A$ via a linear scan.
  3. Return the candidate with count $\ge n/3$.
* **Time Complexity**: Verifying at most 271 candidates takes $271 \times O(n) = \mathbf{O(n)}$ deterministic time. Failure probability is strictly $\le 3^{-100}$.

#### 2. Las Vegas Algorithm [4 Marks]
* A Las Vegas algorithm always returns the correct answer, with a randomized running time.
* **Algorithm**:
  ```text
  Algorithm LasVegasPopular(A, n):
      While TRUE:
          Pick random index i uniformly from {1, 2, ..., n}
          candidate = A[i]

          // Verify candidate frequency
          count = 0
          For j from 1 to n:
              if A[j] == candidate: count++

          If count >= n / 3:
              return candidate
  ```
* **Expected Time Complexity Analysis**:
  Each iteration succeeds with probability $p \ge 1/3$.
  The number of iterations $X$ follows a Geometric Distribution with success probability $p \ge 1/3$.
  $$E[X] = \frac{1}{p} \le \frac{1}{1/3} = 3 \text{ iterations}$$
  Each iteration performs a single linear scan of $A$ in $O(n)$ time.
  $$\text{Expected Time} = E[X] \times O(n) \le 3 \times O(n) = \mathbf{\Theta(n)}$$
  Expected running time is linear $\Theta(n)$.

---

## QUESTION 6 [12 Marks]

### Q.6. (a) Median of $k$ Sorted Arrays of Total Size $n$ [7 Marks]

#### 1. Algorithm Design (Min-Heap Tournament Selection) [4 Marks]
Since all $k$ arrays are pre-sorted and disjoint with total size $n$, the median is the element at rank $m = \lfloor (n + 1)/2 \rfloor$.
1. Construct a Min-Heap of size $k$.
2. Insert the first element of each of the $k$ arrays into the Min-Heap along with its array identifier: `(value, array_index, element_index)`. Building the heap takes $O(k)$ time.
3. For $i$ from 1 to $m$:
   - Extract the minimum element `(val, arr, idx)` from the Min-Heap.
   - If $i == m$, return `val` (Median found).
   - If `idx + 1` is within bounds of array `arr`, insert the next element `(arr[idx+1], arr, idx+1)` into the Min-Heap.
4. Each extraction and insertion takes $O(\log k)$ time.

#### 2. Pseudocode [2 Marks]
```text
Algorithm MedianKSortedArrays(A, k, n):
    m = floor((n + 1) / 2)
    MinHeap H

    For i from 1 to k:
        if A[i] is not empty:
            H.insert( (A[i][0], i, 0) )

    count = 0
    While not H.isEmpty():
        (val, arr, idx) = H.extractMin()
        count = count + 1
        if count == m:
            return val

        if idx + 1 < length(A[arr]):
            H.insert( (A[arr][idx + 1], arr, idx + 1) )
```

#### 3. Time Complexity Analysis [1 Mark]
- Building initial heap of size $k$: $O(k)$.
- Performing $m \approx n/2$ extract-min and insert operations: $\frac{n}{2} \times O(\log k) = O(n \log k)$.
- **Total Time Complexity**: $\mathbf{O(n \log k)}$.
- **Space Complexity**: $\mathbf{O(k)}$ auxiliary space for the heap.
*(Note: Binary search on the coordinate range achieves $O(k \log n \log(\text{MAX}-\text{MIN}))$, but the heap-based selection is strongly polynomial in $O(n \log k)$).*

---

### Q.6. (b) Counting Directed Paths of Length At Most $k$ [5 Marks]

Given directed graph $G = (V, E)$, source $s$, target $t$, and integer $k$. Compute the number of distinct directed paths from $s$ to $t$ having length at most $k$.

#### 1. Dynamic Programming Recurrence [2 Marks]
* Let $P(m, v)$ denote the number of directed paths from source $s$ to vertex $v$ having length **exactly $m$** edges ($0 \le m \le k$).
* A path of length $m$ ending at $v$ must arrive from some in-neighbor $u$ via a directed edge $(u, v) \in E$:
  $$P(m, v) = \sum_{(u, v) \in E} P(m - 1, u) \quad \text{for } 1 \le m \le k$$
* **Base Cases**:
  $$P(0, s) = 1; \quad P(0, v) = 0 \text{ for all } v \ne s$$
* The total number of distinct paths from $s$ to $t$ of length **at most $k$** is:
  $$\text{Total}(s, t, k) = \sum_{m=1}^k P(m, t)$$

#### 2. Algorithm Pseudocode [2 Marks]
```text
Algorithm CountPathsAtMostK(V, E, s, t, k):
    Initialize 2D array P[0..k][1..|V|] to 0
    P[0][s] = 1

    For m from 1 to k:
        For each directed edge (u, v) in E:
            P[m][v] = P[m][v] + P[m - 1][u]

    total_paths = 0
    For m from 1 to k:
        total_paths = total_paths + P[m][t]

    return total_paths
```

#### 3. Complexity Analysis [1 Mark]
* **Time Complexity**:
  - The outer loop runs $k$ times.
  - In each iteration, every edge in $E$ is traversed once.
  - Total Time: $\mathbf{O(k \cdot (|V| + |E|))}$.
* **Space Complexity**:
  - Can be optimized to $\mathbf{O(|V|)}$ by storing only the previous level $P[m-1]$ and current level $P[m]$.

---
**End of Solutions**
