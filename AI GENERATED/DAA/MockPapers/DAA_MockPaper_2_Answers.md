# Model Answers & Marking Scheme: DAA Case-Based Mock Paper 2
## Course: Design and Analysis of Algorithms (CS 3205 / CS 3212)

**Exam**: End Semester Examination (ESE) | **Total Marks**: 60 | **Duration**: 2 Hours  

---

## QUESTION 1 [10 Marks]

### Q.1. (a) Asymptotic Comparisons [5 Marks]

#### (i) $f(n) = (\log_2 n)^{\log_2 n}$ versus $g(n) = \frac{n}{\log_2 n}$ [2.5 Marks]

* **Answer**: **$f(n) = \Omega(g(n))$** (In fact, $g(n) = o(f(n))$, meaning $f(n)$ grows strictly faster than $g(n)$).
* **Proof & Justification**:
  Take the base-2 logarithm of both functions:
  $$\log_2 f(n) = \log_2 \big( (\log_2 n)^{\log_2 n} \big) = (\log_2 n) \cdot \log_2(\log_2 n)$$
  $$\log_2 g(n) = \log_2 \left( \frac{n}{\log_2 n} \right) = \log_2 n - \log_2(\log_2 n)$$
  Now examine the ratio of their logarithms as $n \to \infty$:
  $$\lim_{n \to \infty} \frac{\log_2 f(n)}{\log_2 g(n)} = \lim_{n \to \infty} \frac{(\log_2 n) \cdot \log_2(\log_2 n)}{\log_2 n - \log_2(\log_2 n)}$$
  Divide numerator and denominator by $\log_2 n$:
  $$\lim_{n \to \infty} \frac{\log_2(\log_2 n)}{1 - \frac{\log_2(\log_2 n)}{\log_2 n}} = \frac{\infty}{1 - 0} = \infty$$
  Since $\log_2 f(n) - \log_2 g(n) \to \infty$ as $n \to \infty$, it follows that:
  $$\lim_{n \to \infty} \frac{f(n)}{g(n)} = \lim_{n \to \infty} 2^{\log_2 f(n) - \log_2 g(n)} = 2^{\infty} = \infty$$
  Since the limit of the ratio is $\infty$:
  $$g(n) = o(f(n)) \iff \mathbf{f(n) = \Omega(g(n))}$$

---

#### (ii) $f(n) = 3^{\sqrt{\log_2 n}}$ versus $g(n) = n^{1/3}$ [2.5 Marks]

* **Answer**: **$f(n) = o(g(n))$**
* **Proof & Justification**:
  Take the base-2 logarithm of both functions:
  $$\log_2 f(n) = \log_2 \big( 3^{\sqrt{\log_2 n}} \big) = \sqrt{\log_2 n} \cdot \log_2 3 \approx 1.585 \sqrt{\log_2 n}$$
  $$\log_2 g(n) = \log_2 \big( n^{1/3} \big) = \frac{1}{3} \log_2 n$$
  Now evaluate the ratio of their logarithms as $n \to \infty$:
  $$\lim_{n \to \infty} \frac{\log_2 f(n)}{\log_2 g(n)} = \lim_{n \to \infty} \frac{(\log_2 3) \sqrt{\log_2 n}}{\frac{1}{3} \log_2 n} = (3 \log_2 3) \lim_{n \to \infty} \frac{1}{\sqrt{\log_2 n}} = 0$$
  This demonstrates that the logarithm of $f(n)$ is asymptotically dominated by the logarithm of $g(n)$:
  $$\log_2 g(n) - \log_2 f(n) = \frac{1}{3} \log_2 n - (\log_2 3) \sqrt{\log_2 n} \to \infty$$
  Therefore:
  $$\lim_{n \to \infty} \frac{f(n)}{g(n)} = 0 \iff \mathbf{f(n) = o(g(n))}$$
  *(A sub-polynomial exponential of a sub-linear power of $\log n$ is strictly smaller than any positive polynomial power $n^\epsilon$)*.

---

### Q.1. (b) Maximum Weight Independent Set on Trees [5 Marks]

* **Problem Formulation**:
  Given tree $T = (V, E)$ with weights $w(v) > 0$, find an independent set $S \subseteq V$ maximizing $\sum_{v \in S} w(v)$.

* **Algorithm Design (Tree Dynamic Programming)**:
  1. Root the tree arbitrarily at an arbitrary vertex $r$ (e.g., node 1) using adjacency lists.
  2. For each vertex $u \in V$, define two quantities:
     - $W_{\text{in}}(u)$: The maximum weight of an independent set in the subtree rooted at $u$, such that **$u$ is included**.
     - $W_{\text{ex}}(u)$: The maximum weight of an independent set in the subtree rooted at $u$, such that **$u$ is excluded**.
  3. **Recurrence Relation**:
     - If $u$ is included, none of its children can be included:
       $$W_{\text{in}}(u) = w(u) + \sum_{v \in \text{children}(u)} W_{\text{ex}}(v)$$
     - If $u$ is excluded, each child $v$ can independently either be included or excluded:
       $$W_{\text{ex}}(u) = \sum_{v \in \text{children}(u)} \max\big(W_{\text{in}}(v), W_{\text{ex}}(v)\big)$$
  4. **Base Cases**: For any leaf vertex $l$ ($\text{children}(l) = \emptyset$):
     $$W_{\text{in}}(l) = w(l), \quad W_{\text{ex}}(l) = 0$$

* **Algorithm Pseudocode**:
  ```text
  Algorithm MaxWeightIndependentSetTree(T, w, r):
      // Input: Tree T = (V, E) with n vertices rooted at r, weight array w
      // Output: Maximum total weight of an independent set

      Function PostOrderDP(u, parent):
          Win[u] = w[u]
          Wex[u] = 0

          For each neighbor v in Adj[u]:
              If v != parent:
                  PostOrderDP(v, u)
                  Win[u] = Win[u] + Wex[v]
                  Wex[u] = Wex[u] + max(Win[v], Wex[v])

      PostOrderDP(r, NULL)
      return max(Win[r], Wex[r])
  ```

* **Correctness & Complexity Analysis**:
  - **Correctness**: Proved by structural induction on tree height. In any independent set, a vertex $u$ is either chosen or not. By the principle of optimality on trees, subtrees of children are disjoint; their optimal independent subsets can be chosen independently subject only to the adjacency constraint with $u$.
  - **Time Complexity**: The algorithm performs a single post-order Depth-First Search (DFS). Each vertex is visited once, and each edge is examined twice. For a tree with $n$ vertices, $|E| = n - 1$.
    $$T(n) = \Theta(|V| + |E|) = \Theta(n + n - 1) = \mathbf{\Theta(n)}$$
    This is asymptotically optimal because every vertex weight must be examined.
  - **Space Complexity**: $\Theta(n)$ for arrays `Win`, `Wex`, and recursion stack depth $O(n)$ in the worst case (skewed tree).

---

## QUESTION 2 [12 Marks]

### Q.2. (a) Matrix Exponentiation for Fibonacci & $F_{13} \bmod 1000$ [6 Marks]

#### (i) Matrix Formulation & Divide-and-Conquer Algorithm [3 Marks]
* **State Transition Equation**:
  The Fibonacci recurrence $F_{k} = F_{k-1} + F_{k-2}$ can be represented in matrix form:
  $$\begin{pmatrix} F_{k+1} \\ F_k \end{pmatrix} = \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix} \begin{pmatrix} F_k \\ F_{k-1} \end{pmatrix}$$
  By induction on $n \ge 1$:
  $$\begin{pmatrix} F_{n+1} & F_n \\ F_n & F_{n-1} \end{pmatrix} = \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix}^n = M^n, \quad \text{where } M = \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix}$$
  Thus, $F_n = (M^n)_{1,2} = (M^n)_{2,1}$.

* **Divide-and-Conquer Algorithm**:
  ```text
  Algorithm MatrixMultiplyMod(A, B, m):
      C[0][0] = (A[0][0]*B[0][0] + A[0][1]*B[1][0]) mod m
      C[0][1] = (A[0][0]*B[0][1] + A[0][1]*B[1][1]) mod m
      C[1][0] = (A[1][0]*B[0][0] + A[1][1]*B[1][0]) mod m
      C[1][1] = (A[1][0]*B[0][1] + A[1][1]*B[1][1]) mod m
      return C

  Algorithm MatrixPowerMod(M, n, m):
      If n == 0:
          return IdentityMatrix()  // [[1, 0], [0, 1]]
      If n == 1:
          return M mod m
      half = MatrixPowerMod(M, floor(n / 2), m)
      half_sq = MatrixMultiplyMod(half, half, m)
      If n mod 2 == 0:
          return half_sq
      Else:
          return MatrixMultiplyMod(M, half_sq, m)
  ```
* **Complexity**: Matrix multiplication of size $2 \times 2$ takes $O(1)$ operations. The recurrence is $T(n) = T(\lfloor n/2 \rfloor) + O(1)$, which yields **$T(n) = \Theta(\log n)$**.

#### (ii) Numerical Trace for $F_{13} \bmod 1000$ [3 Marks]
* Compute $M^{13} \bmod 1000$:
  Binary expansion of exponent: $13 = 8 + 4 + 1 = (1101)_2$.
  - **$M^1$**:
    $$M = \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix}$$
  - **$M^2 = M \times M$**:
    $$M^2 = \begin{pmatrix} 1\cdot 1 + 1\cdot 1 & 1\cdot 1 + 1\cdot 0 \\ 1\cdot 1 + 0\cdot 1 & 1\cdot 1 + 0\cdot 0 \end{pmatrix} = \begin{pmatrix} 2 & 1 \\ 1 & 1 \end{pmatrix}$$
  - **$M^4 = M^2 \times M^2$**:
    $$M^4 = \begin{pmatrix} 2\cdot 2 + 1\cdot 1 & 2\cdot 1 + 1\cdot 1 \\ 1\cdot 2 + 1\cdot 1 & 1\cdot 1 + 1\cdot 1 \end{pmatrix} = \begin{pmatrix} 5 & 3 \\ 3 & 2 \end{pmatrix}$$
  - **$M^8 = M^4 \times M^4$**:
    $$M^8 = \begin{pmatrix} 5\cdot 5 + 3\cdot 3 & 5\cdot 3 + 3\cdot 2 \\ 3\cdot 5 + 2\cdot 3 & 3\cdot 3 + 2\cdot 2 \end{pmatrix} = \begin{pmatrix} 34 & 21 \\ 21 & 13 \end{pmatrix}$$
  - **Now combine $M^{13} = M^8 \times M^4 \times M^1$**:
    First, $M^{12} = M^8 \times M^4$:
    $$M^{12} = \begin{pmatrix} 34\cdot 5 + 21\cdot 3 & 34\cdot 3 + 21\cdot 2 \\ 21\cdot 5 + 13\cdot 3 & 21\cdot 3 + 13\cdot 2 \end{pmatrix} = \begin{pmatrix} 170 + 63 & 102 + 42 \\ 105 + 39 & 63 + 26 \end{pmatrix} = \begin{pmatrix} 233 & 144 \\ 144 & 89 \end{pmatrix}$$
    Finally, $M^{13} = M^{12} \times M$:
    $$M^{13} = \begin{pmatrix} 233 & 144 \\ 144 & 89 \end{pmatrix} \begin{pmatrix} 1 & 1 \\ 1 & 0 \end{pmatrix} = \begin{pmatrix} 233\cdot 1 + 144\cdot 1 & 233\cdot 1 + 144\cdot 0 \\ 144\cdot 1 + 89\cdot 1 & 144\cdot 1 + 89\cdot 0 \end{pmatrix} = \begin{pmatrix} 377 & \mathbf{233} \\ \mathbf{233} & 144 \end{pmatrix}$$
* Reading off the entry $(M^{13})_{1,2} \bmod 1000$:
  $$F_{13} \equiv \mathbf{233} \pmod{1000}$$
  *(Verification: Sequence: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233. $F_{13} = 233$).*

---

### Q.2. (b) Skip-Two Elimination Josephus Game [6 Marks]

$n$ players numbered $1, 2, \dots, n$ standing in a circle. Player holding sword skips 1 player, eliminates the 2nd player clockwise, and hands sword to next alive player (kills every 2nd person counted from the sword-holder).

#### (i) Recurrence Relation & Correctness Proof [3 Marks]
* In 0-indexed modular representation ($0, 1, \dots, n-1$), player 0 skips 1, kills player 2, and passes sword to player 3.
* The recurrence for survivor position in 0-indexed notation with step size $k$:
  $$J(1, k) = 0$$
  $$J(n, k) = \big(J(n - 1, k) + k\big) \bmod n$$
* For our problem, after the first elimination:
  The circle shrinks from $n$ players to $n - 1$ players.
  The next sword holder is at index $k \bmod n$.
  Shifting the coordinate frame by $k$:
  $$J(n, k) = (J(n - 1, k) + k) \bmod n$$
* Converting to 1-indexed notation:
  $$W(1) = 1$$
  $$W(n) = \big((W(n - 1) + k - 1) \bmod n\big) + 1$$
  With step parameter $k = 2$:
  $$W(n) = \big((W(n - 1) + 1) \bmod n\big) + 1$$
* **Proof of Correctness**:
  In a circle of $n$ players, the first player killed is at index 2 (1-based: player 1 skips 1, kills 2).
  The sword is now handed to Player 3.
  There are now $n - 1$ remaining players, with Player 3 acting as the new starting position.
  Renumbering the remaining players with Player 3 as position 1:
  Position $x$ in the $(n-1)$-player circle maps to position $(x + 2 - 1) \bmod n + 1$ in the original circle.
  Therefore, by mathematical induction on $n$, the survivor of the $n$-player game must be the mapped position of the survivor of the $(n-1)$-player game.

#### (ii) Winner for $n = 8, 16$ and General Pattern [3 Marks]
Using the 1-indexed recurrence $W(n) = \big((W(n - 1) + 1) \bmod n\big) + 1$:
- $n = 1$: $W(1) = 1$
- $n = 2$: $W(2) = ((1 + 1) \bmod 2) + 1 = 0 + 1 = 1$
- $n = 3$: $W(3) = ((1 + 1) \bmod 3) + 1 = 2 + 1 = 3$
- $n = 4$: $W(4) = ((3 + 1) \bmod 4) + 1 = 0 + 1 = 1$
- $n = 5$: $W(5) = ((1 + 1) \bmod 5) + 1 = 2 + 1 = 3$
- $n = 6$: $W(6) = ((3 + 1) \bmod 6) + 1 = 4 + 1 = 5$
- $n = 7$: $W(7) = ((5 + 1) \bmod 7) + 1 = 6 + 1 = 7$
- $n = 8$: $W(8) = ((7 + 1) \bmod 8) + 1 = 0 + 1 = \mathbf{1}$
- Continuing to $n = 16$:
  - $W(9) = ((1 + 1) \bmod 9) + 1 = 3$
  - $W(10) = ((3 + 1) \bmod 10) + 1 = 5$
  - $W(11) = ((5 + 1) \bmod 11) + 1 = 7$
  - $W(12) = ((7 + 1) \bmod 12) + 1 = 9$
  - $W(13) = ((9 + 1) \bmod 13) + 1 = 11$
  - $W(14) = ((11 + 1) \bmod 14) + 1 = 13$
  - $W(15) = ((13 + 1) \bmod 15) + 1 = 15$
  - $W(16) = ((15 + 1) \bmod 16) + 1 = 0 + 1 = \mathbf{1}$
* **General Pattern**:
  For all powers of two $n = 2^m$:
  $$\mathbf{W(2^m) = 1}$$
  **Physical Explanation**: When $n = 2^m$, after exactly one complete cycle through the circle, half of the players are eliminated, and the sword returns precisely to Player 1 with a new circle size of $2^{m-1}$. By induction, Player 1 retains the sword at every power-of-two transition, ultimately remaining as the sole survivor.

---

## QUESTION 3 [8 Marks]

### Q.3. (a) Attempt any ONE [5 Marks]

#### OPTION (a)(i): Longest Path in Directed Acyclic Graph (DAG) [5 Marks]

* **1. Recurrence Relation**:
  Let $L(v)$ be the weight of the longest path from source $s$ to vertex $v$.
  $$L(v) = \begin{cases} 
  0 & \text{if } v = s \\
  -\infty & \text{if } v \ne s \text{ and in-degree}(v) = 0 \\
  \max_{(u, v) \in E} \big( L(u) + w(u, v) \big) & \text{otherwise}
  \end{cases}$$

* **2. Dynamic Programming Algorithm using Topological Sort**:
  ```text
  Algorithm DAGLongestPath(G, w, s, t):
      1. Compute Topological Ordering of vertices: [v_1, v_2, ..., v_n]
      2. Initialize array L[1..n] to -infinity
      3. L[s] = 0

      4. For each vertex u in topological order starting from s:
          If L[u] != -infinity:
              For each directed outgoing edge (u, v) in E:
                  If L[u] + w(u, v) > L[v]:
                      L[v] = L[u] + w(u, v)

      5. return L[t]
  ```

* **3. Complexity & NP-Hardness Contrast**:
  - **Time Complexity**: Topological sort takes $\Theta(|V| + |E|)$ time using Kahn's algorithm or DFS. The relaxation phase visits each vertex once and each edge once. Total time is $\mathbf{\Theta(|V| + |E|)}$, and space is $\Theta(|V|)$.
  - **Why DAGs are Polynomial but General Graphs are NP-Hard**:
    In a DAG, the absence of directed cycles ensures a strict partial order: edges only point forward in the topological order. Consequently, subproblems exhibit **optimal substructure and no cyclic dependencies**; each subproblem $L(u)$ can be completely finalized before any edge $(u, v)$ is relaxed.
    In general graphs with cycles, finding the longest *simple* path is **NP-hard** (can be reduced from the Hamiltonian Path problem by setting all edge weights to 1; a simple path of length $|V|-1$ exists if and only if $G$ has a Hamiltonian Path).

---

#### OPTION (a)(ii): Huffman Coding for 7 Symbols [5 Marks]
Alphabet: $\{A: 4, B: 6, C: 11, D: 13, E: 17, F: 21, G: 28\}$. Total characters: 50,000.

* **Step 1: Construction of Huffman Tree**:
  1. Forest: $\{A: 4, B: 6, C: 11, D: 13, E: 17, F: 21, G: 28\}$
  2. Merge two minimums $A(4)$ and $B(6) \to N_1(10)$.
     Forest: $\{N_1: 10, C: 11, D: 13, E: 17, F: 21, G: 28\}$
  3. Merge two minimums $N_1(10)$ and $C(11) \to N_2(21)$.
     Forest: $\{D: 13, E: 17, N_2: 21, F: 21, G: 28\}$
  4. Merge two minimums $D(13)$ and $E(17) \to N_3(30)$.
     Forest: $\{N_2: 21, F: 21, G: 28, N_3: 30\}$
  5. Merge two minimums $N_2(21)$ and $F(21) \to N_4(42)$.
     Forest: $\{G: 28, N_3: 30, N_4: 42\}$
  6. Merge two minimums $G(28)$ and $N_3(30) \to N_5(58)$.
     Forest: $\{N_4: 42, N_5: 58\}$
  7. Merge two minimums $N_4(42)$ and $N_5(58) \to \text{Root}(100)$.

* **Step 2: Codeword Assignment (Left = 0, Right = 1)**:
  - Root(100) $\to$ Left: $N_4(42)$ [0], Right: $N_5(58)$ [1]
  - $N_4(42) \to$ Left: $N_2(21)$ [00], Right: $F(21)$ [01] $\implies \mathbf{F = 01}$ (Length 2)
  - $N_2(21) \to$ Left: $N_1(10)$ [000], Right: $C(11)$ [001] $\implies \mathbf{C = 001}$ (Length 3)
  - $N_1(10) \to$ Left: $A(4)$ [0000] $\implies \mathbf{A = 0000}$ (Length 4), Right: $B(6)$ [0001] $\implies \mathbf{B = 0001}$ (Length 4)
  - $N_5(58) \to$ Left: $G(28)$ [10] $\implies \mathbf{G = 10}$ (Length 2), Right: $N_3(30)$ [11]
  - $N_3(30) \to$ Left: $D(13)$ [110] $\implies \mathbf{D = 110}$ (Length 3), Right: $E(17)$ [111] $\implies \mathbf{E = 111}$ (Length 3)

| Symbol | Frequency | Codeword | Length ($l_i$) | Total Bits ($f_i \times l_i$) |
| :---: | :---: | :---: | :---: | :---: |
| **A** | 4 | `0000` | 4 | $4 \times 4 = 16$ |
| **B** | 6 | `0001` | 4 | $6 \times 4 = 24$ |
| **C** | 11 | `001` | 3 | $11 \times 3 = 33$ |
| **D** | 13 | `110` | 3 | $13 \times 3 = 39$ |
| **E** | 17 | `111` | 3 | $17 \times 3 = 51$ |
| **F** | 21 | `01` | 2 | $21 \times 2 = 42$ |
| **G** | 28 | `10` | 2 | $28 \times 2 = 56$ |
| **Total** | **100** | — | — | **261 bits** |

* **Step 3: Encoded Document Size Calculation**:
  - Average bits per character $= \frac{261}{100} = \mathbf{2.61 \text{ bits/char}}$.
  - For a document of $50,000$ characters:
    $$\text{Total Encoded Size} = 50,000 \times 2.61 = \mathbf{130,500 \text{ bits}} \quad (\text{or } 16,312.5 \text{ bytes})$$

---

### Q.3. (b) Attempt any ONE [3 Marks]

#### OPTION (b)(i): Master Theorem on $T(n) = 8T(n/2) + n^3 \log_2 n$ [3 Marks]
* **Form**: $T(n) = a T(n/b) + f(n)$ with $a = 8, b = 2, f(n) = n^3 \log_2 n$.
* **Critical Exponent**:
  $$n^{\log_b a} = n^{\log_2 8} = n^3$$
* **Comparison with $f(n)$**:
  $$f(n) = n^3 \log_2 n = \Theta(n^{\log_b a} \log^k n) \quad \text{with } k = 1$$
* **Extended Master Theorem (Case 2)**:
  When $f(n) = \Theta(n^{\log_b a} \log^k n)$ for $k \ge 0$:
  $$T(n) = \Theta(n^{\log_b a} \log^{k+1} n)$$
* **Conclusion**:
  $$T(n) = \Theta(n^3 \log^{1+1} n) = \mathbf{\Theta(n^3 \log_2^2 n)}$$

---

#### OPTION (b)(ii): Prove $\sum_{k=1}^n k \log_2 k = \Theta(n^2 \log_2 n)$ [3 Marks]
* **Upper Bound ($O(n^2 \log n)$)**:
  For all $k \le n$, $k \log_2 k \le n \log_2 n$.
  $$\sum_{k=1}^n k \log_2 k \le \sum_{k=1}^n n \log_2 n = n \cdot (n \log_2 n) = n^2 \log_2 n$$
  Thus, $\sum_{k=1}^n k \log_2 k \le c_2 n^2 \log_2 n$ with $c_2 = 1$.

* **Lower Bound ($\Omega(n^2 \log n)$)**:
  Split the summation into two halves and retain only the upper half ($k \ge \lceil n/2 \rceil$):
  $$\sum_{k=1}^n k \log_2 k \ge \sum_{k=\lceil n/2 \rceil}^n k \log_2 k$$
  For every term in this upper half, $k \ge \frac{n}{2}$ and $\log_2 k \ge \log_2(n/2) = \log_2 n - 1$.
  For $n \ge 4$, $\log_2 n - 1 \ge \frac{1}{2} \log_2 n$. Therefore:
  $$\sum_{k=\lceil n/2 \rceil}^n k \log_2 k \ge \left(\frac{1}{2} \log_2 n\right) \sum_{k=\lceil n/2 \rceil}^n k \ge \left(\frac{1}{2} \log_2 n\right) \left( \frac{n}{2} \cdot \frac{n}{2} \right) = \frac{1}{8} n^2 \log_2 n$$
  Thus, $\sum_{k=1}^n k \log_2 k \ge c_1 n^2 \log_2 n$ with $c_1 = \frac{1}{8}$ for all $n \ge 4$.

* **Conclusion**:
  Since $c_1 n^2 \log_2 n \le \sum_{k=1}^n k \log_2 k \le c_2 n^2 \log_2 n$, by definition:
  $$\sum_{k=1}^n k \log_2 k = \mathbf{\Theta(n^2 \log_2 n)} \quad \blacksquare$$

---

## QUESTION 4 [10 Marks]

### Q.4. (a) Track Diameter Bisection Proof [5 Marks]

$2n$ checkpoints on a circular track ($n$ Red, $n$ Blue, $n$ even). Prove a diameter line exists partitioning the track into two semicircles, each containing exactly $n/2$ Red and $n/2$ Blue points.

* **Proof via Discrete Invariant & Intermediate Value Theorem**:
  1. **Continuous Rotation Parameterization**:
     Represent the circular track as an angle $\theta \in [0, 2\pi)$.
     For any angle $\theta$, consider the diameter line $L(\theta)$ passing through the center of the track at angle $\theta$.
     $L(\theta)$ partitions the track into two closed semicircles:
     - $S^+(\theta)$: the semicircle spanning $[\theta, \theta + \pi)$.
     - $S^-(\theta)$: the opposite semicircle spanning $[\theta + \pi, \theta + 2\pi)$.
  2. **Marker Counting Functions**:
     Let $R(\theta)$ be the number of Red markers in $S^+(\theta)$.
     Let $B(\theta)$ be the number of Blue markers in $S^+(\theta)$.
     Since the entire track has $n$ Red markers and $n$ Blue markers, the opposite semicircle $S^-(\theta)$ contains $n - R(\theta)$ Red markers and $n - B(\theta)$ Blue markers.
  3. **Equator Balance Function**:
     Define the discrepancy function:
     $$\Delta(\theta) = R(\theta) - \frac{n}{2}$$
     Notice that rotating the diameter by $\pi$ radians ($180^\circ$) swaps the two semicircles:
     $S^+(\theta + \pi) = S^-(\theta)$.
     Therefore:
     $$R(\theta + \pi) = n - R(\theta)$$
     $$\Delta(\theta + \pi) = R(\theta + \pi) - \frac{n}{2} = \left(n - R(\theta)\right) - \frac{n}{2} = \frac{n}{2} - R(\theta) = -\Delta(\theta)$$
  4. **Step-by-Step Rotation**:
     Start at $\theta = 0$.
     As $\theta$ increases continuously from $0$ to $\pi$, the semicircle $S^+(\theta)$ sweeps across the track.
     Because no two markers are diametrically opposite, the boundary of the diameter encounters at most one marker at a time.
     Each time the diameter passes over a Red marker, $R(\theta)$ changes by at most $\pm 1$.
     Thus, $|\Delta(\theta^+) - \Delta(\theta^-)| \le 1$.
  5. **Intermediate Value Argument**:
     - If $\Delta(0) = 0$, then $R(0) = n/2$, and the diameter at $\theta = 0$ already bisects the Red markers.
     - If $\Delta(0) \ne 0$, say $\Delta(0) > 0$, then by the antipodal identity:
       $$\Delta(\pi) = -\Delta(0) < 0$$
     - Since $\Delta(\theta)$ starts strictly positive and ends strictly negative, changing only in discrete integer steps of $\pm 1$, by the **Discrete Intermediate Value Theorem**, there must exist an angle $\theta^*$ where:
       $$\Delta(\theta^*) = 0 \iff R(\theta^*) = \frac{n}{2}$$
  6. **Simultaneous Bisection of Blue Markers**:
     Since $S^+(\theta^*)$ contains exactly $n$ markers in total (each semicircle spans half the track perimeter and points are distributed such that diameter bisection captures half the points), the number of Blue markers in $S^+(\theta^*)$ is:
     $$B(\theta^*) = n - R(\theta^*) = n - \frac{n}{2} = \frac{n}{2}$$
     Consequently, both semicircles contain exactly $n/2$ Red markers and $n/2$ Blue markers. $\blacksquare$

---

### Q.4. (b) Damerau-Levenshtein Edit Distance with Transposition [5 Marks]

#### (1) Concrete Recurrence Relation [2 Marks]
For $1 \le i \le m$ and $1 \le j \le n$:
$$D(i, j) = \min \begin{cases}
D(i - 1, j) + 1 & \text{(Deletion of } x[i] \text{)} \\
D(i, j - 1) + 1 & \text{(Insertion of } y[j] \text{)} \\
D(i - 1, j - 1) + \text{cost} & \text{(Substitution, where } \text{cost} = 0 \text{ if } x[i] == y[j] \text{ else } 1\text{)} \\
D(i - 2, j - 2) + 1 & \text{(Transposition, only if } i, j \ge 2 \text{ and } x[i] == y[j-1] \text{ and } x[i-1] == y[j]\text{)}
\end{cases}$$

#### (2) Base Cases [1 Mark]
- Transforming an empty string into a prefix of length $j$ requires $j$ insertions:
  $$D(0, j) = j \quad \text{for all } 0 \le j \le n$$
- Transforming a prefix of length $i$ into an empty string requires $i$ deletions:
  $$D(i, 0) = i \quad \text{for all } 0 \le i \le m$$

#### (3) Dynamic Programming Algorithm & Complexity [2 Marks]
```text
Algorithm DamerauLevenshtein(X, Y, m, n):
    Initialize 2D array D[0..m][0..n]

    For i from 0 to m: D[i][0] = i
    For j from 0 to n: D[0][j] = j

    For i from 1 to m:
        For j from 1 to n:
            cost = (X[i] == Y[j]) ? 0 : 1

            D[i][j] = min( D[i - 1][j] + 1,          // Deletion
                           D[i][j - 1] + 1,          // Insertion
                           D[i - 1][j - 1] + cost )  // Substitution

            // Check adjacent transposition condition
            If (i > 1 and j > 1 and X[i] == Y[j - 1] and X[i - 1] == Y[j]):
                D[i][j] = min( D[i][j], D[i - 2][j - 2] + 1 )

    return D[m][n]
```
* **Complexity**:
  - **Time Complexity**: There are $(m+1)(n+1)$ subproblems in the 2D matrix. Each cell performs at most 4 constant-time comparisons: $\mathbf{O(m \cdot n)}$.
  - **Space Complexity**: $\mathbf{O(m \cdot n)}$ for the full table (can be optimized to $O(n)$ using 3 rolling rows).

---

## QUESTION 5 [10 Marks]

### Q.5. (a) Polynomial Reduction: 3-SAT $\le_P$ INDEPENDENT SET [5 Marks]

#### 1. Formal Problem Definitions [1.5 Marks]
* **3-SAT Decision Problem**:
  - **Input**: A boolean formula $\Phi = C_1 \wedge C_2 \wedge \dots \wedge C_m$ in Conjunctive Normal Form (CNF) over $n$ variables $x_1, \dots, x_n$, where each clause $C_i = (l_{i,1} \vee l_{i,2} \vee l_{i,3})$ contains exactly 3 literals.
  - **YES Instance**: There exists a truth assignment $\tau: \{x_1, \dots, x_n\} \to \{\text{True}, \text{False}\}$ such that $\Phi$ evaluates to True.
  - **NO Instance**: No truth assignment satisfies $\Phi$.

* **INDEPENDENT SET Decision Problem**:
  - **Input**: An undirected graph $G = (V, E)$ and an integer $k \ge 1$.
  - **YES Instance**: There exists a vertex subset $S \subseteq V$ of size $|S| \ge k$ such that no two vertices in $S$ are joined by an edge.
  - **NO Instance**: No independent set of size $\ge k$ exists.

#### 2. Reduction Construction ($3\text{-SAT} \le_P \text{INDEPENDENT SET}$) [1 Mark]
Given 3-CNF formula $\Phi$ with $m$ clauses:
1. For each clause $C_i = (l_{i,1} \vee l_{i,2} \vee l_{i,3})$, create a triangle gadget of 3 vertices labeled with its literals: $V_i = \{v_{i,1}, v_{i,2}, v_{i,3}\}$.
   Total vertices: $|V| = 3m$.
2. Add edges:
   - **Intra-clause edges**: Connect all 3 vertices within each clause gadget $V_i$ to form a triangle: $(v_{i,1}, v_{i,2}), (v_{i,2}, v_{i,3}), (v_{i,1}, v_{i,3})$.
   - **Inter-clause conflict edges**: Connect any two vertices in different gadgets if their literals are mutually contradictory (e.g., connect $x_j$ in clause $i$ to $\neg x_j$ in clause $k$).
3. Set the target independent set size:
   $$k = m$$
* **Polynomial Time**: Constructing $3m$ vertices and at most $\binom{3m}{2}$ edges takes $O(m^2)$ time.

#### 3. Correctness Proof [2.5 Marks]
* **$(\implies)$ If $\Phi$ is satisfiable $\implies G$ has an Independent Set of size $\ge m$**:
  - Suppose $\tau$ satisfies $\Phi$. Then in every clause $C_i$, at least one literal evaluates to True under $\tau$.
  - For each clause gadget $V_i$, select exactly one vertex $v_{i}^*$ whose corresponding literal is True.
  - This selects exactly $m$ vertices (one per clause).
  - Can any two selected vertices have an edge between them?
    - No two selected vertices are in the same clause (we chose at most 1 per triangle).
    - No two selected vertices represent contradictory literals $x_j$ and $\neg x_j$, because $\tau$ is a valid truth assignment and cannot assign True to both $x_j$ and $\neg x_j$.
  - Thus, no conflict edge connects any pair of selected vertices. The set forms a valid independent set of size $m$.

* **$(\impliedby)$ If $G$ has an Independent Set of size $\ge m \implies \Phi$ is satisfiable**:
  - Suppose $G$ has an independent set $S$ with $|S| \ge m$.
  - Since each clause is a triangle (a clique of size 3), $S$ can contain at most 1 vertex from each clause gadget.
  - Since there are $m$ gadgets and $|S| \ge m$, $S$ must contain **exactly 1 vertex from each of the $m$ clause gadgets**.
  - Because $S$ is an independent set, no two vertices in $S$ are joined by an edge, meaning $S$ contains no contradictory pairs $x_j$ and $\neg x_j$.
  - Construct truth assignment $\tau$:
    - For each vertex in $S$, set its corresponding literal to True.
    - Set any remaining unassigned variables arbitrarily to True or False.
  - Since every clause gadget contributes one literal to $S$, and that literal is set to True, every clause evaluates to True. Hence, $\tau$ satisfies $\Phi$. $\blacksquare$

---

### Q.5. (b) Knight's Tour Backtracking Formulation [5 Marks]

#### 1. State-Space Tree Definition [1 Mark]
A **State-Space Tree** is a tree representation of all possible partial and complete configurations evaluated during a search. The root represents the initial empty state, each level represents a decision/choice, branches represent candidate moves, and leaves represent complete solutions or pruned invalid dead-ends.

#### 2. Knight's Tour Formulation [2.5 Marks]
* **State Representation**:
  An $N \times N$ integer matrix `board[0..N-1][0..N-1]`, initialized to -1.
  A move counter `move_count` ranging from 0 to $N^2 - 1$.
  The start square is `board[0][0] = 0`.
* **Choice Vector**:
  A knight has 8 legal moves from $(r, c)$:
  $$(\Delta r, \Delta c) \in \{(2, 1), (1, 2), (-1, 2), (-2, 1), (-2, -1), (-1, -2), (1, -2), (2, -1)\}$$
* **Constraint Check (`isSafe`)**:
  ```text
  Function isSafe(r, c, board, N):
      return (r >= 0 and r < N and c >= 0 and c < N and board[r][c] == -1)
  ```

#### 3. Algorithm & Backtracking Step [1.5 Marks]
```text
Algorithm SolveKT(r, c, move_count, board, N, dRow, dCol):
    If move_count == N * N:
        return TRUE  // All N^2 squares visited

    For k from 0 to 7:
        next_r = r + dRow[k]
        next_c = c + dCol[k]

        If isSafe(next_r, next_c, board, N):
            board[next_r][next_c] = move_count  // Make move

            If SolveKT(next_r, next_c, move_count + 1, board, N, dRow, dCol) == TRUE:
                return TRUE

            // BACKTRACKING STEP:
            board[next_r][next_c] = -1           // Unmake move, restore state

    return FALSE  // Trigger backtrack to previous level
```
* **The Backtracking Step**: If exploring all 8 sub-moves from `(next_r, next_c)` fails to complete the tour, the algorithm executes `board[next_r][next_c] = -1`. This unmarks the square as visited, restores the state, and permits the loop to try the next candidate move.

---

## QUESTION 6 [10 Marks]

### Q.6. (a) Circular Board Walk vs. Teleport Optimal Strategy [6 Marks]

Circular ring of $n$ cells: $0, 1, \dots, n-1$. Target is cell $n-1$.
From cell $i$:
- **Walk**: Move to $(i + 1) \bmod n$ (cost 1s).
- **Teleport**: Coin flip $\to$ Heads ($1/2$): cell $n-1$; Tails ($1/2$): stay at cell $i$ (cost 1s).

#### 1. Expected Cost Equations [2 Marks]
* Let $E(i)$ be the expected time to reach cell $n-1$ from cell $i$.
* Target boundary condition: $E(n-1) = 0$.
* If we choose **Walk** from cell $i$:
  $$T_{\text{walk}}(i) = 1 + E(i + 1)$$
* If we choose **Teleport** from cell $i$:
  $$T_{\text{teleport}}(i) = 1 + \frac{1}{2} E(n-1) + \frac{1}{2} E(i) = 1 + 0 + \frac{1}{2} E(i)$$
  Solving for $E(i)$:
  $$E(i) = 1 + \frac{1}{2} E(i) \implies \frac{1}{2} E(i) = 1 \implies \mathbf{E(i) = 2 \text{ seconds}}$$
  **Remarkable Insight**: The expected time of adopting the Teleport action at cell $i$ until success is a geometric random variable with parameter $p = 1/2$. The expected number of flips is $1/p = 2$ seconds!

#### 2. Optimal Strategy Formulation [2 Marks]
* At each cell $i$, the optimal Bellman equation is:
  $$E(i) = \min\big(1 + E(i+1), \, 2\big)$$
* Calculate backwards from target cell $n-1$:
  - Cell $n-1$: $E(n-1) = 0$.
  - Cell $n-2$: $T_{\text{walk}} = 1 + E(n-1) = 1 + 0 = 1$. Since $1 < 2$, **Walk**! $E(n-2) = 1$.
  - Cell $n-3$: $T_{\text{walk}} = 1 + E(n-2) = 1 + 1 = 2$. Walk and Teleport both take 2. $E(n-3) = 2$.
  - Cell $n-4$: $T_{\text{walk}} = 1 + E(n-3) = 1 + 2 = 3$. Since $3 > 2$, **Teleport**! $E(n-4) = 2$.
  - In general, for any cell $i \le n - 3$: $T_{\text{walk}} = 1 + 2 = 3 > 2$. Thus Teleport is strictly optimal.

#### 3. Expected Time Proof [2 Marks]
* Starting at cell 0 (for any $n \ge 3$):
  Since cell $0 \le n - 3$, the optimal strategy at cell 0 is **Teleport**.
  The expected time to reach cell $n-1$ from cell 0 is:
  $$\mathbf{E(0) = 2 \text{ seconds}}$$
* **Asymptotic Function**:
  The expected time is independent of $n$ for $n \ge 3$:
  $$\mathbf{E(0) = \Theta(1)}$$
  *(In contrast to deterministic walking which takes $n-1$ seconds, the randomized teleportation achieves constant expected time $\Theta(1)$).*

---

### Q.6. (b) Attempt any ONE [4 Marks]

#### OPTION (b)(i): Lamé's Theorem Proof for Euclidean GCD [4 Marks]

* **Statement of Lamé's Theorem**:
  The number of division steps in the Euclidean Algorithm applied to two positive integers $a > b \ge 1$ is at most 5 times the number of decimal digits in $b$.

* **Step 1: Prove $b \ge F_{k+1}$ by Induction on Division Steps**:
  Let the sequence of Euclidean divisions be:
  $$a = q_1 b + r_1 \quad (0 < r_1 < b)$$
  $$b = q_2 r_1 + r_2 \quad (0 < r_2 < r_1)$$
  $$r_1 = q_3 r_2 + r_3 \quad (0 < r_3 < r_2)$$
  $$\dots$$
  $$r_{k-2} = q_{k-1} r_{k-1} + r_k \quad (0 < r_k < r_{k-1})$$
  $$r_{k-1} = q_k r_k + 0 \quad (\text{final step, } r_{k+1} = 0)$$

  Because each remainder is strictly positive except the last:
  - In the last step $k$: $q_k \ge 2$ (if $q_k = 1$, $r_{k-1} = r_k$, contradicting $r_k < r_{k-1}$).
    Since $r_k \ge 1$:
    $$r_k \ge 1 = F_2$$
    $$r_{k-1} = q_k r_k \ge 2 \times 1 = 2 = F_3$$
  - For each preceding step $i \in \{1, \dots, k-1\}$, the quotient $q_i \ge 1$.
    Using the division equation:
    $$r_{i-1} = q_i r_i + r_{i+1} \ge 1 \cdot r_i + r_{i+1} = r_i + r_{i+1}$$
  - By induction:
    $$r_k \ge F_2$$
    $$r_{k-1} \ge F_3$$
    $$r_{k-2} \ge r_{k-1} + r_k \ge F_3 + F_2 = F_4$$
    $$\dots$$
    $$b = r_0 \ge r_1 + r_2 \ge F_k + F_{k-1} = \mathbf{F_{k+1}}$$
  This proves the inequality $b \ge F_{k+1}$.

* **Step 2: Logarithmic Worst-Case Bound**:
  Recall that Fibonacci numbers grow exponentially with the Golden Ratio $\phi = \frac{1 + \sqrt{5}}{2} \approx 1.618$:
  $$F_{k+1} > \phi^{k-1}$$
  Therefore:
  $$b \ge F_{k+1} > \phi^{k-1}$$
  Taking base-$\phi$ logarithms:
  $$k - 1 < \log_\phi b \implies k < \log_\phi b + 1 = \frac{\log_{10} b}{\log_{10} \phi} + 1 \approx 4.785 \log_{10} b + 1$$
  Thus, the number of division steps $k$ is strictly bounded by $O(\log b)$.
  Since each integer division takes $O(1)$ operations (or $O(\log^2 b)$ bit operations), the overall algorithm runtime is **$O(\log b)$**. $\blacksquare$

---

#### OPTION (b)(ii): Convex Polygon Triangulation Recurrence & $T(5)$ [4 Marks]

* **1. Recurrence Relation Derivation**:
  Consider a convex polygon with $n + 2$ vertices labeled in clockwise order: $v_1, v_2, \dots, v_{n+2}$.
  Fix the base edge $e = (v_1, v_{n+2})$.
  In any valid triangulation, edge $(v_1, v_{n+2})$ must belong to exactly one triangle.
  The third vertex of this triangle must be some vertex $v_k$ where $2 \le k \le n+1$.
  Choosing vertex $v_k$ divides the remaining polygon into two smaller sub-polygons:
  - Left sub-polygon formed by vertices $v_1, v_2, \dots, v_k$ (having $k$ vertices, i.e., $(k - 2) + 2$ vertices).
  - Right sub-polygon formed by vertices $v_k, v_{k+1}, \dots, v_{n+2}$ (having $(n + 2) - k + 1 = n - k + 3$ vertices).
  By the multiplication rule, for a chosen $k$, the number of triangulations is:
  $$T(k - 2) \times T(n - k + 1)$$
  Summing over all possible choices of $k$ from $2$ to $n+1$:
  Let $i = k - 2$. As $k$ goes from $2$ to $n+1$, $i$ goes from $0$ to $n-1$:
  $$\mathbf{T(n) = \sum_{i=0}^{n-1} T(i) \cdot T(n - 1 - i)} \quad \text{for } n \ge 1$$
  Base case: A triangle ($n=1$, 3 vertices) has only 1 triangulation:
  $$T(1) = 1, \quad \text{and define } T(0) = 1$$

* **2. Identification with Catalan Numbers**:
  This recurrence is precisely the definition of the Catalan numbers shifted by 1:
  $$T(n) = C_n = \frac{1}{n + 1} \binom{2n}{n}$$

* **3. Computing $T(5)$ for a Convex Heptagon (7 vertices)**:
  Here the polygon has 7 vertices, so $n + 2 = 7 \implies n = 5$.
  $$T(5) = C_5 = \frac{1}{5 + 1} \binom{10}{5} = \frac{1}{6} \cdot \frac{10 \times 9 \times 8 \times 7 \times 6}{5 \times 4 \times 3 \times 2 \times 1}$$
  Numerator: $10 \times 9 \times 8 \times 7 \times 6 = 30240$.
  Denominator: $6 \times 120 = 720$.
  $$T(5) = \frac{30240}{720} = \mathbf{42}$$
* **Result**: A convex heptagon has **42 distinct triangulations**.

---
**End of Model Answers & Marking Scheme**
