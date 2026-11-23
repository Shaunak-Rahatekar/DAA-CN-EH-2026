# Solutions & Marking Scheme: VIT Pune End-Semester Examination
## Course: Design and Analysis of Algorithms (CS 3205 / CS 3212)

**Exam**: End-Semester Examination (ESE) | **Branch**: Computer Engineering (TY)  
**Maximum Marks**: 60 | **Duration**: 2 Hours  

---

## Instructions & Exam Characteristics Analysis
1. **Mathematical Rigor & Formality**: Informal, conversational arguments receive no credit. Proofs must use induction, contradiction, or explicit invariants.
2. **Implementation Details**: Only fundamental data structures (arrays, linked lists, simple trees) are permitted without full implementation details.
3. **Question Structure**: 6 Questions, total 60 marks:
   - **Q1**: 10 Marks (Asymptotics [5M] + Tree Graph Algorithm / Independent Set [5M])
   - **Q2**: 12 Marks (Modular Exponentiation [6M] + Josephus Recurrence & Closed-Form [6M])
   - **Q3**: 8 Marks (Shortest Path DP OR Huffman Coding [5M] + Master Theorem OR Harmonic Number Bound [3M])
   - **Q4**: 10 Marks (Necklace/Bracelet Bisection Proof [5M] + Palindrome Insertion DP [5M])
   - **Q5**: 10 Marks (Vertex Cover to Clique Polynomial Reduction [5M] + Sudoku Backtracking [5M])
   - **Q6**: 10 Marks (Randomized Directed Path Step vs. Jump Strategy [6M] + Fermat Coprimality Proof OR Catalan Tree Recurrence [4M])

---

## QUESTION 1 [10 Marks]

### Q.1. (a) Asymptotic Comparisons [5 Marks]

Consider following pair of functions $f(n)$ and $g(n)$. You need to choose correct options amongst $f(n) = o(g(n))$ or $f(n) = \Theta(g(n))$ or $f(n) = \Omega(g(n))$, where $o, \Theta, \Omega$ are small-oh, Big-Theta, Big-Omega asymptotic notations. Give justification for your answer.

#### (i) $f(n) = n^{\log n}$ and $g(n) = 2^n$ [2.5 Marks]

* **Answer**: **$f(n) = o(g(n))$**
* **Proof & Justification**:
  To compare $f(n) = n^{\log_2 n}$ and $g(n) = 2^n$, we analyze the ratio $\frac{f(n)}{g(n)}$ as $n \to \infty$.
  Take the base-2 logarithm of both functions:
  $$\log_2 f(n) = \log_2 \left(n^{\log_2 n}\right) = (\log_2 n) \cdot (\log_2 n) = (\log_2 n)^2$$
  $$\log_2 g(n) = \log_2 \left(2^n\right) = n$$
  Now examine the ratio of their logarithms:
  $$\lim_{n \to \infty} \frac{\log_2 f(n)}{\log_2 g(n)} = \lim_{n \to \infty} \frac{(\log_2 n)^2}{n}$$
  Applying L'Hôpital's Rule (differentiating numerator and denominator with respect to $n$):
  $$\lim_{n \to \infty} \frac{2 (\log_2 n) \cdot \frac{1}{n \ln 2}}{1} = \lim_{n \to \infty} \frac{2 \log_2 n}{n \ln 2}$$
  Applying L'Hôpital's Rule a second time:
  $$\lim_{n \to \infty} \frac{\frac{2}{n \ln 2}}{\ln 2} = \lim_{n \to \infty} \frac{2}{n (\ln 2)^2} = 0$$
  Since $\log_2 f(n) - \log_2 g(n) = (\log_2 n)^2 - n \to -\infty$, it follows that:
  $$\lim_{n \to \infty} \frac{f(n)}{g(n)} = \lim_{n \to \infty} 2^{(\log_2 n)^2 - n} = 2^{-\infty} = 0$$
  By definition of small-oh notation:
  $$\lim_{n \to \infty} \frac{f(n)}{g(n)} = 0 \iff f(n) = o(g(n))$$

---

#### (ii) $f(n) = \log \left(\frac{\sqrt{n}}{5}\right)$ and $g(n) = \log n$ [2.5 Marks]

* **Answer**: **$f(n) = \Theta(g(n))$**
* **Proof & Justification**:
  Using logarithmic identities, simplify $f(n)$:
  $$f(n) = \log \left(\frac{\sqrt{n}}{5}\right) = \log(n^{1/2}) - \log 5 = \frac{1}{2} \log n - \log 5$$
  Now evaluate the limit of the ratio $\frac{f(n)}{g(n)}$ as $n \to \infty$:
  $$\lim_{n \to \infty} \frac{f(n)}{g(n)} = \lim_{n \to \infty} \frac{\frac{1}{2} \log n - \log 5}{\log n} = \lim_{n \to \infty} \left( \frac{1}{2} - \frac{\log 5}{\log n} \right)$$
  Since $\log 5$ is a constant and $\log n \to \infty$ as $n \to \infty$, we have $\lim_{n \to \infty} \frac{\log 5}{\log n} = 0$. Therefore:
  $$\lim_{n \to \infty} \frac{f(n)}{g(n)} = \frac{1}{2}$$
  Since the limit evaluates to a strictly positive, finite constant $c = \frac{1}{2} \in (0, \infty)$, there exist positive constants $c_1 = \frac{1}{4}$, $c_2 = 1$, and $n_0 = 25$ such that for all $n \ge n_0$:
  $$c_1 g(n) \le f(n) \le c_2 g(n)$$
  By definition, **$f(n) = \Theta(g(n))$**.

---

### Q.1. (b) Maximum Blue Coloring on Trees (Independent Set) [5 Marks]

Suppose you are given a tree with $n$ vertices (tree is a graph without any cycles). We want to color maximum possible vertices of the tree with color blue so that no two adjacent vertices are colored blue. Give an efficient algorithm for this task. What is the complexity of your algorithm? For full credit, time complexity of your algorithm must be asymptotically equal to the time complexity of the best algorithm for this task.

* **Problem Formulation**:
  This is the **Maximum Independent Set (MIS)** problem specialized to trees. While MIS is NP-hard on general graphs, on trees it admits an optimal linear-time $\Theta(n)$ dynamic programming algorithm.

* **Algorithm Design (Tree Dynamic Programming)**:
  1. Root the given tree $T$ arbitrarily at vertex $r$ (e.g., node 1).
  2. For each vertex $u \in V$, define two quantities:
     - $I(u)$: The maximum number of blue vertices in the subtree rooted at $u$, given that **$u$ is colored blue**.
     - $E(u)$: The maximum number of blue vertices in the subtree rooted at $u$, given that **$u$ is NOT colored blue**.
  3. **Recurrence Relation**:
     - If $u$ is colored blue, none of its immediate children can be colored blue (to satisfy the adjacency constraint):
       $$I(u) = 1 + \sum_{v \in \text{children}(u)} E(v)$$
     - If $u$ is not colored blue, each child $v$ can independently either be colored blue or not:
       $$E(u) = \sum_{v \in \text{children}(u)} \max\big(I(v), E(v)\big)$$
  4. **Base Case**: For any leaf node $l$ ($\text{children}(l) = \emptyset$):
     $$I(l) = 1, \quad E(l) = 0$$

* **Algorithm Pseudocode**:
  ```text
  Algorithm MaxBlueTree(T, r):
      // Input: Tree T = (V, E) with n vertices, rooted at r
      // Output: Maximum number of non-adjacent blue vertices

      Function PostOrderDP(u, parent):
          I[u] = 1
          E[u] = 0

          For each neighbor v of u:
              If v != parent:
                  PostOrderDP(v, u)
                  I[u] = I[u] + E[v]
                  E[u] = E[u] + max(I[v], E[v])

      PostOrderDP(r, NULL)
      return max(I[r], E[r])
  ```

* **Complexity Analysis**:
  - **Time Complexity**: The algorithm performs a single depth-first / post-order traversal of the tree. Every vertex is visited once, and each edge $(u, v)$ is traversed exactly twice (once down, once up). Since a tree with $n$ vertices has exactly $n - 1$ edges:
    $$T(n) = \Theta(|V| + |E|) = \Theta(n + n - 1) = \mathbf{\Theta(n)}$$
    This is asymptotically optimal because any algorithm must inspect all $n$ vertices.
  - **Space Complexity**: $\Theta(n)$ auxiliary space for arrays $I$ and $E$, plus recursion stack depth $O(n)$ in the worst case (skewed tree).

---

## QUESTION 2 [12 Marks]

### Q.2. (a) Fast Modular Exponentiation & Compute $3^{83} \bmod 17$ [6 Marks]

Given natural numbers $a, b, n$, give a recursive algorithm to compute $a^b \bmod n$. Time complexity of your algorithm must be logarithmic in $b$. Use your algorithm or any other method to find $3^{83} \bmod 17$.

* **Recursive Algorithm (Repeated Squaring / Binary Exponentiation)**:
  - **Mathematical Basis**:
    $$a^b \bmod n = \begin{cases} 
    1 \bmod n & \text{if } b = 0 \\
    \big((a^{b/2} \bmod n)^2\big) \bmod n & \text{if } b > 0 \text{ and } b \text{ is even} \\
    \Big( (a \bmod n) \cdot \big((a^{(b-1)/2} \bmod n)^2 \bmod n\big) \Big) \bmod n & \text{if } b > 0 \text{ and } b \text{ is odd}
    \end{cases}$$

* **Algorithm Pseudocode**:
  ```text
  Algorithm ModExp(a, b, n):
      If b == 0:
          return 1 mod n
      half = ModExp(a, floor(b / 2), n)
      half_sq = (half * half) mod n
      If (b mod 2 == 0):
          return half_sq
      Else:
          return ( (a mod n) * half_sq ) mod n
  ```

* **Time Complexity**:
  At each recursive call, the exponent $b$ is halved: $T(b) = T(\lfloor b/2 \rfloor) + \Theta(1)$.
  By the Master Theorem, $T(b) = \mathbf{\Theta(\log b)}$ multiplications and modulo operations.

* **Numerical Trace: Finding $3^{83} \bmod 17$**:
  - *Method 1: Repeated Squaring Trace*:
    Convert exponent $83$ to binary: $83 = 64 + 16 + 2 + 1 = (1010011)_2$.
    Compute powers of 3 modulo 17:
    - $3^1 \equiv 3 \pmod{17}$
    - $3^2 \equiv 3^2 = 9 \pmod{17}$
    - $3^4 \equiv 9^2 = 81 = 4 \times 17 + 13 \equiv 13 \equiv -4 \pmod{17}$
    - $3^8 \equiv (-4)^2 = 16 \equiv -1 \pmod{17}$
    - $3^{16} \equiv (-1)^2 = 1 \pmod{17}$
    - $3^{32} \equiv 1^2 = 1 \pmod{17}$
    - $3^{64} \equiv 1^2 = 1 \pmod{17}$
    Now combine:
    $$3^{83} = 3^{64} \cdot 3^{16} \cdot 3^2 \cdot 3^1 \pmod{17}$$
    $$3^{83} \equiv (1) \cdot (1) \cdot (9) \cdot (3) \equiv 27 \equiv \mathbf{10} \pmod{17}$$

  - *Method 2: Verification via Fermat's Little Theorem*:
    Since $p = 17$ is prime and $\gcd(3, 17) = 1$:
    $$3^{16} \equiv 1 \pmod{17}$$
    Divide the exponent 83 by 16: $83 = 16 \times 5 + 3$.
    $$3^{83} = (3^{16})^5 \cdot 3^3 \equiv (1)^5 \cdot 27 \equiv 27 \equiv \mathbf{10} \pmod{17}$$

---

### Q.2. (b) Josephus Game Analysis [6 Marks]

Consider Josephus game with $n$ players numbered 1 to $n$. Player 1 kills player 2 and passes sword to player 3. Player 3 kills player 4 and passes sword to 5... Player holding sword kills next alive person in clockwise order. Let $f(n)$ denote the winner.

#### (i) Recurrence Relation for $f(n)$ & Correctness Proof [3 Marks]
* **Recurrence**:
  - Base Case: $f(1) = 1$
  - Even $n$ ($n = 2k$):
    $$f(2k) = 2f(k) - 1$$
  - Odd $n$ ($n = 2k + 1$):
    $$f(2k + 1) = 2f(k) + 1$$

* **Proof of Correctness**:
  - **Case 1: $n = 2k$ (Even)**:
    In the first round, players $2, 4, 6, \dots, 2k$ are killed in order.
    The sword returns to Player 1. Exactly $k$ players survive: $\{1, 3, 5, \dots, 2k - 1\}$.
    This is now a game of $k$ players starting from the original Player 1.
    A player at new index $x \in \{1, 2, \dots, k\}$ corresponds to the original player numbered $2x - 1$.
    Therefore, the winner is:
    $$f(2k) = 2f(k) - 1$$
  - **Case 2: $n = 2k + 1$ (Odd)**:
    In the first round, players $2, 4, 6, \dots, 2k$ are killed.
    Then Player $2k + 1$ holds the sword and **kills Player 1**, passing the sword to Player 3.
    Exactly $k$ players survive: $\{3, 5, 7, \dots, 2k + 1\}$.
    This is now a game of $k$ players starting with Player 3 as the leader (new position 1).
    A player at new index $x \in \{1, 2, \dots, k\}$ corresponds to the original player numbered $2x + 1$.
    Therefore, the winner is:
    $$f(2k + 1) = 2f(k) + 1$$

#### (ii) Winner for $n = 2^m + 1$ Players [1.5 Marks]
* Any integer $n$ can be written uniquely as $n = 2^m + l$, where $2^m$ is the largest power of 2 such that $2^m \le n$, and $0 \le l < 2^m$.
* The closed-form solution to the recurrence is:
  $$f(2^m + l) = 2l + 1$$
* Here, $n = 2^m + 1$, which gives $l = 1$.
  $$f(2^m + 1) = 2(1) + 1 = \mathbf{3}$$
* **Physical Justification**: After Player 1 kills Player 2, exactly 1 player is eliminated, leaving $2^m$ players. The sword is passed to Player 3. Whenever a game begins with a power-of-two number of players, the player who starts that round wins. Since Player 3 holds the sword at the moment the circle size becomes $2^m$, **Player 3** is the winner.

#### (iii) Winner for $n = 2^m - 1$ Players [1.5 Marks]
* Express $n = 2^m - 1$ in the form $2^k + l$:
  The largest power of 2 strictly less than or equal to $2^m - 1$ is $2^{m-1}$.
  $$n = 2^{m-1} + (2^{m-1} - 1)$$
  Thus, the highest power of 2 has exponent $m-1$, and the remainder is $l = 2^{m-1} - 1$.
* Substituting into the closed-form formula:
  $$f(n) = 2l + 1 = 2(2^{m-1} - 1) + 1 = 2^m - 2 + 1 = \mathbf{2^m - 1} = n$$
* **Result**: The winner is **Player $2^m - 1$** (the last player in the circle).

---

## QUESTION 3 [8 Marks]

### Q.3. (a) Attempt any ONE [5 Marks]

#### OPTION (i): Shortest Path with At Most $k$ Edges [5 Marks]
Let $G(V, E)$ be a weighted directed graph without negative weight cycles. $f(s, t, k)$ denotes the weight of the smallest weight path from $s$ to $t$ amongst all paths of length at most $k$.

* **1. Concrete Recurrence Relation**:
  $$f(s, t, k) = \min \left\{ f(s, t, k - 1), \min_{(u, t) \in E} \big( f(s, u, k - 1) + w(u, t) \big) \right\}$$
  *Explanation*: A path from $s$ to $t$ of at most $k$ edges either:
  - Uses at most $k-1$ edges (weight $f(s, t, k-1)$), OR
  - Uses exactly $k$ edges, meaning it reaches some in-neighbor $u$ of $t$ in at most $k-1$ edges, followed by the edge $(u, t)$.

* **2. Base Cases**:
  For $k = 0$:
  $$f(s, t, 0) = \begin{cases} 0 & \text{if } t = s \\ \infty & \text{if } t \ne s \end{cases}$$

* **3. Dynamic Programming Algorithm (Bellman-Ford Formulation)**:
  ```text
  Algorithm BoundedEdgeShortestPath(G, w, s, t):
      n = |V|
      // DP Table: dp[k][v] represents f(s, v, k)
      Initialize dp[0..n-1][1..n] to infinity
      dp[0][s] = 0

      For k from 1 to n - 1:
          For each vertex v in V:
              dp[k][v] = dp[k - 1][v]
          For each directed edge (u, v) in E:
              if dp[k - 1][u] + w(u, v) < dp[k][v]:
                  dp[k][v] = dp[k - 1][u] + w(u, v)

      return dp[n - 1][t]
  ```

* **4. Complexity**:
  - Since $G$ has no negative weight cycles, any shortest simple path has at most $|V| - 1$ edges. Thus $k$ ranges from $0$ to $|V|-1$.
  - In each outer iteration $k$, every edge $e \in E$ is relaxed once.
  - **Time Complexity**: $\mathbf{O(|V| \cdot |E|)}$.
  - **Space Complexity**: $O(|V|^2)$, or optimized to $O(|V|)$ using two rows ($k$ and $k-1$).

---

#### OPTION (ii): Huffman Coding for 7 Characters [5 Marks]
Alphabet: $\{a, b, c, d, e, f, g\}$, frequencies: $5, 7, 8, 16, 18, 22, 24$. Total characters $= 100,000$.

* **Step 1: Huffman Tree Construction**:
  1. Priority queue of frequencies:
     $$Q = \{a: 5, b: 7, c: 8, d: 16, e: 18, f: 22, g: 24\}$$
  2. Extract two minimums: $a(5)$ and $b(7)$. Create node $N_1(12)$.
     $$Q = \{c: 8, N_1: 12, d: 16, e: 18, f: 22, g: 24\}$$
  3. Extract two minimums: $c(8)$ and $N_1(12)$. Create node $N_2(20)$.
     $$Q = \{d: 16, e: 18, N_2: 20, f: 22, g: 24\}$$
  4. Extract two minimums: $d(16)$ and $e(18)$. Create node $N_3(34)$.
     $$Q = \{N_2: 20, f: 22, g: 24, N_3: 34\}$$
  5. Extract two minimums: $N_2(20)$ and $f(22)$. Create node $N_4(42)$.
     $$Q = \{g: 24, N_3: 34, N_4: 42\}$$
  6. Extract two minimums: $g(24)$ and $N_3(34)$. Create node $N_5(58)$.
     $$Q = \{N_4: 42, N_5: 58\}$$
  7. Extract two minimums: $N_4(42)$ and $N_5(58)$. Create Root $R(100)$.

* **Step 2: Codeword Assignment (Left = 0, Right = 1)**:
  - Root(100) $\to$ Left: $N_4(42)$ [prefix 0], Right: $N_5(58)$ [prefix 1]
  - $N_4(42) \to$ Left: $N_2(20)$ [00], Right: $f(22)$ [01] $\implies \mathbf{f = 01}$ (Length 2)
  - $N_2(20) \to$ Left: $c(8)$ [000] $\implies \mathbf{c = 000}$ (Length 3), Right: $N_1(12)$ [001]
  - $N_1(12) \to$ Left: $a(5)$ [0010] $\implies \mathbf{a = 0010}$ (Length 4), Right: $b(7)$ [0011] $\implies \mathbf{b = 0011}$ (Length 4)
  - $N_5(58) \to$ Left: $g(24)$ [10] $\implies \mathbf{g = 10}$ (Length 2), Right: $N_3(34)$ [11]
  - $N_3(34) \to$ Left: $d(16)$ [110] $\implies \mathbf{d = 110}$ (Length 3), Right: $e(18)$ [111] $\implies \mathbf{e = 111}$ (Length 3)

| Character | Relative Frequency | Codeword | Codeword Length ($l_i$) | Bits per 100 chars ($f_i \cdot l_i$) |
| :---: | :---: | :---: | :---: | :---: |
| **a** | 5 | `0010` | 4 | $5 \times 4 = 20$ |
| **b** | 7 | `0011` | 4 | $7 \times 4 = 28$ |
| **c** | 8 | `000` | 3 | $8 \times 3 = 24$ |
| **d** | 16 | `110` | 3 | $16 \times 3 = 48$ |
| **e** | 18 | `111` | 3 | $18 \times 3 = 54$ |
| **f** | 22 | `01` | 2 | $22 \times 2 = 44$ |
| **g** | 24 | `10` | 2 | $24 \times 2 = 48$ |
| **Total** | **100** | — | — | **266 bits** |

* **Step 3: Encoded Text Size Calculation**:
  - Average bits per character $= \frac{266}{100} = 2.66 \text{ bits/char}$.
  - Total text length $= 100,000$ characters.
  - Total encoded size $= 100,000 \times 2.66 = \mathbf{266,000 \text{ bits}}$ (or $33,250 \text{ bytes}$).

---

### Q.3. (b) Attempt any ONE [3 Marks]

#### OPTION (i): Master Theorem on $T(n) = 4T(n/2) + n^2$ [3 Marks]
* **Form**: $T(n) = a T(n/b) + f(n)$ where $a = 4, b = 2, f(n) = n^2$.
* **Critical Exponent**:
  $$n^{\log_b a} = n^{\log_2 4} = n^2$$
* **Comparison**:
  Compare $f(n)$ with $n^{\log_b a}$:
  $$f(n) = n^2 = \Theta(n^{\log_b a})$$
* **Case Determination**:
  This matches **Case 2** of the Master Theorem where $f(n) = \Theta(n^{\log_b a} \log^k n)$ with $k = 0$.
* **Conclusion**:
  $$T(n) = \Theta(n^{\log_b a} \log^{k+1} n) = \mathbf{\Theta(n^2 \log n)}$$

---

#### OPTION (ii): Prove $H_n = O(\log n)$ [3 Marks]
* **Definition**: $H_n = 1 + \frac{1}{2} + \frac{1}{3} + \dots + \frac{1}{n} = \sum_{k=1}^n \frac{1}{k}$.
* **Proof by Integral Bounding**:
  Consider the function $f(x) = \frac{1}{x}$, which is strictly continuous and monotonically decreasing for all $x \ge 1$.
  For any integer $k \ge 2$:
  $$\frac{1}{k} \le \int_{k-1}^k \frac{1}{x} \, dx$$
  Summing both sides from $k = 2$ to $n$:
  $$\sum_{k=2}^n \frac{1}{k} \le \sum_{k=2}^n \int_{k-1}^k \frac{1}{x} \, dx = \int_1^n \frac{1}{x} \, dx = \ln n$$
  Now add the first term ($k = 1$):
  $$H_n = 1 + \sum_{k=2}^n \frac{1}{k} \le 1 + \ln n$$
  Using change of base, $\ln n = \frac{\log_2 n}{\log_2 e} \approx 0.693 \log_2 n$.
  For all $n \ge 2$, $1 \le \ln n$, which gives:
  $$H_n \le 1 + \ln n \le 2 \ln n \le (2 \ln 2) \log_2 n$$
  Choosing $c = 2$ and $n_0 = 2$, we have $H_n \le c \log n$ for all $n \ge n_0$.
  Therefore, **$H_n = O(\log n)$**. $\blacksquare$

---

## QUESTION 4 [10 Marks]

### Q.4. (a) 2-Cut Bracelet Bisection Proof [5 Marks]

A bracelet is made up of $2n$ black and $2n$ white beads. Prove that we can cut a portion of the bracelet that has exactly $n$ white and $n$ black beads. (Partition into two contiguous portions by cutting at two positions).

* **Proof (Discrete Intermediate Value Theorem)**:
  1. **Circular Representation**:
     Represent the circular bracelet as a sequence of $4n$ beads indexed clockwise as $1, 2, \dots, 4n$.
     The total number of beads is $4n$, with exactly $2n$ white beads and $2n$ black beads.
  2. **Contiguous Arc Formulation**:
     Any two cuts that isolate a contiguous portion of $2n$ beads also partition the bracelet into two complementary contiguous portions, each of length exactly $2n$.
     For each $i \in \{1, 2, \dots, 4n\}$, let $A(i)$ denote the contiguous semicircular arc of $2n$ beads starting at bead $i$ and ending at bead $(i + 2n - 1) \bmod 4n$.
  3. **Counting Function**:
     Let $W(i)$ be the number of white beads in arc $A(i)$.
     The number of black beads in arc $A(i)$ is $2n - W(i)$.
     Notice that arc $A(i)$ and arc $A(i + 2n)$ are disjoint and together contain all $4n$ beads of the bracelet.
     Since the entire bracelet contains exactly $2n$ white beads in total:
     $$W(i) + W(i + 2n) = 2n \iff W(i + 2n) = 2n - W(i)$$
  4. **Difference Metric**:
     Define the difference function $\Delta(i) = W(i) - n$.
     Then:
     $$\Delta(i + 2n) = W(i + 2n) - n = (2n - W(i)) - n = n - W(i) = -\Delta(i)$$
  5. **Discrete Continuity (Step Size Property)**:
     Observe how $W(i)$ changes when we advance from arc $A(i)$ to arc $A(i+1)$:
     $$A(i+1) = \big( A(i) \setminus \{ \text{bead } i \} \big) \cup \{ \text{bead } i + 2n \}$$
     Arc $A(i+1)$ drops one bead (bead $i$) and gains one bead (bead $i + 2n$).
     Therefore, the white bead count can change by at most 1:
     $$|W(i+1) - W(i)| \le 1 \implies |\Delta(i+1) - \Delta(i)| \le 1$$
  6. **Application of Discrete Intermediate Value Theorem**:
     - If $\Delta(1) = 0$, then $W(1) = n$. Arc $A(1)$ already has exactly $n$ white beads and $n$ black beads.
     - If $\Delta(1) \ne 0$, without loss of generality assume $\Delta(1) > 0$.
     - Then by anti-symmetry: $\Delta(1 + 2n) = -\Delta(1) < 0$.
     - $\Delta(i)$ starts at a strictly positive value $\Delta(1) > 0$ and ends at a strictly negative value $\Delta(1 + 2n) < 0$, changing by at most $\pm 1$ at each integer step.
     - Therefore, $\Delta(i)$ **must cross zero**. There exists some index $i^* \in \{1, 2, \dots, 2n\}$ such that:
       $$\Delta(i^*) = 0 \iff W(i^*) = n$$
  7. **Conclusion**:
     The contiguous arc $A(i^*)$ contains exactly $n$ white beads, and consequently $2n - n = n$ black beads.
     Cutting the bracelet at position $i^*$ and position $(i^* + 2n)$ yields two contiguous portions, each containing exactly $n$ white and $n$ black beads. $\blacksquare$

---

### Q.4. (b) Minimum Character Insertions for Palindrome [5 Marks]

Given string $s = s[1..n]$, find minimum characters to insert to make $s$ a palindrome. Example: $s = \text{"Ab3bd"} \to 2$ insertions ($\text{"dAb3bAd"}$). Let $f(i, j)$ be minimum insertions for substring $s[i..j]$.

#### (1) Clean Recurrence Relation for $f(i, j)$ [2 Marks]
For $1 \le i \le j \le n$:
$$f(i, j) = \begin{cases}
0 & \text{if } i \ge j \\
f(i + 1, j - 1) & \text{if } s[i] == s[j] \\
1 + \min\big(f(i + 1, j), \, f(i, j - 1)\big) & \text{if } s[i] \ne s[j]
\end{cases}$$
*Explanation*: If the boundary characters match ($s[i] == s[j]$), they naturally form symmetric outer brackets of the palindrome, requiring 0 extra insertions. If they mismatch, we insert a character matching $s[i]$ at the right boundary (leaving $s[i+1..j]$ to be solved) or matching $s[j]$ at the left boundary (leaving $s[i..j-1]$ to be solved), incurring 1 insertion.

#### (2) Base Cases [1 Mark]
- For all $i$: $f(i, i) = 0$ (a single character is trivially a palindrome).
- For all $i > j$: $f(i, j) = 0$ (an empty string is trivially a palindrome).
- For adjacent characters ($j = i + 1$):
  $$f(i, i+1) = \begin{cases} 0 & \text{if } s[i] == s[i+1] \\ 1 & \text{if } s[i] \ne s[i+1] \end{cases}$$

#### (3) Dynamic Programming Algorithm & Complexity [2 Marks]
* **Algorithm (Bottom-Up Tabulation)**:
  ```text
  Algorithm MinInsertionsPalindrome(s, n):
      Initialize dp[1..n][1..n] to 0

      // Substring length len from 2 to n
      For len from 2 to n:
          For i from 1 to n - len + 1:
              j = i + len - 1
              If s[i] == s[j]:
                  dp[i][j] = dp[i + 1][j - 1]
              Else:
                  dp[i][j] = 1 + min(dp[i + 1][j], dp[i][j - 1])

      return dp[1][n]
  ```

* **Trace for Example $s = \text{"Ab3bd"}$**:
  - Length $n = 5$.
  - Longest Palindromic Subsequence (LPS) of "Ab3bd" is "b3b" of length 3.
  - Formula: $\text{Min Insertions} = n - \text{Length of LPS} = 5 - 3 = \mathbf{2}$.
  - The 2 inserted characters are `'d'` at index 0 and `'A'` at index 6, forming `"dAb3bAd"`.

* **Complexity**:
  - **Time Complexity**: $\mathbf{O(n^2)}$ (a table of $\frac{n(n+1)}{2}$ subproblems, each solved in $O(1)$ time).
  - **Space Complexity**: $\mathbf{O(n^2)}$ for the 2D DP table (can be optimized to $O(n)$ space using two rows).

---

## QUESTION 5 [10 Marks]

### Q.5. (a) Polynomial Reduction: VERTEX COVER $\le_P$ CLIQUE [5 Marks]

Precisely state VERTEX COVER and CLIQUE (Input, YES/NO instances). Give polynomial-time reduction from VERTEX COVER to CLIQUE and prove correctness.

#### 1. Formal Problem Definitions [1.5 Marks]
* **VERTEX COVER Decision Problem**:
  - **Input**: An undirected graph $G = (V, E)$ and an integer $k \ge 1$.
  - **YES Instance**: There exists a subset $V' \subseteq V$ of size $|V'| \le k$ such that for every edge $(u, v) \in E$, at least one endpoint is in $V'$ (i.e., $u \in V'$ or $v \in V'$).
  - **NO Instance**: No such subset of size $\le k$ exists.

* **CLIQUE Decision Problem**:
  - **Input**: An undirected graph $H = (V_H, E_H)$ and an integer $k' \ge 1$.
  - **YES Instance**: There exists a subset $C \subseteq V_H$ of size $|C| \ge k'$ such that for all distinct pairs $u, v \in C$, $(u, v) \in E_H$ (every pair of vertices in $C$ is connected by an edge).
  - **NO Instance**: No such subset of size $\ge k'$ exists.

#### 2. Reduction Construction ($\text{VERTEX COVER} \le_P \text{CLIQUE}$) [1 Mark]
Given an instance $\langle G = (V, E), k \rangle$ of VERTEX COVER:
1. Construct the **complement graph** $\overline{G} = (V, \overline{E})$, where:
   $$\overline{E} = \{ (u, v) \mid u \ne v \text{ and } (u, v) \notin E \}$$
2. Set the target clique size:
   $$k' = |V| - k$$
3. Output the instance $\langle \overline{G}, k' \rangle$ for the CLIQUE problem.
* **Polynomial-Time Check**: Constructing $\overline{E}$ examines all $\binom{|V|}{2}$ vertex pairs and takes $O(|V|^2)$ time, which is strictly polynomial in input size.

#### 3. Correctness Proof [2.5 Marks]
We must prove that:
$$G \text{ has a vertex cover of size } \le k \iff \overline{G} \text{ has a clique of size } \ge |V| - k$$

* **$(\implies)$ Forward Direction**:
  - Suppose $G$ has a vertex cover $V' \subseteq V$ with $|V'| \le k$.
  - Define $C = V \setminus V'$. Then:
    $$|C| = |V| - |V'| \ge |V| - k$$
  - We show $C$ is a clique in $\overline{G}$.
  - Take any two distinct vertices $u, v \in C$.
  - Since $u, v \notin V'$ and $V'$ is a vertex cover for $G$, the edge $(u, v)$ CANNOT belong to $E$ (if $(u, v) \in E$, at least one of $u$ or $v$ would have to be in $V'$, which is a contradiction).
  - By definition of the complement graph, since $(u, v) \notin E$, we must have $(u, v) \in \overline{E}$.
  - Since this holds for every pair in $C$, $C$ is a clique in $\overline{G}$ of size $\ge |V| - k$.

* **$(\impliedby)$ Backward Direction**:
  - Suppose $\overline{G}$ has a clique $C \subseteq V$ with $|C| \ge |V| - k$.
  - Define $V' = V \setminus C$. Then:
    $$|V'| = |V| - |C| \le |V| - (|V| - k) = k$$
  - We show $V'$ is a vertex cover for $G$.
  - Suppose for contradiction that $V'$ is NOT a vertex cover for $G$.
  - Then there exists some edge $(u, v) \in E$ such that neither $u \in V'$ nor $v \in V'$.
  - This means both $u \in C$ and $v \in C$.
  - Since $C$ is a clique in $\overline{G}$, every pair of vertices in $C$ must have an edge in $\overline{G}$, meaning $(u, v) \in \overline{E}$.
  - However, an edge cannot exist simultaneously in both $E$ and $\overline{E}$ by definition of graph complement ($\overline{E} \cap E = \emptyset$). Contradiction!
  - Therefore, $V'$ covers every edge in $E$, so $V'$ is a valid vertex cover of size $\le k$. $\blacksquare$

---

### Q.5. (b) Backtracking & Sudoku Solver Algorithm [5 Marks]

What is backtracking? Give a backtracking based algorithm for solving the standard SUDOKU puzzle. Describe clearly the backtracking step.

#### 1. Definition of Backtracking [1.5 Marks]
* **Backtracking** is a refined, recursive depth-first search strategy for solving Constraint Satisfaction Problems (CSPs).
* Instead of generating all candidate configurations (brute force), backtracking constructs partial solutions incrementally.
* At each step, a **bounding/pruning function** evaluates whether the partial solution can possibly lead to a valid complete solution. If constraints are violated, the algorithm prunes the entire subtree, **undoes the tentative choice (the backtracking step)**, and reverts the state to explore sibling branches.

#### 2. Sudoku Problem Formulation & Algorithm [2.5 Marks]
* A standard Sudoku is a $9 \times 9$ grid partitioned into nine $3 \times 3$ subgrids.
* Empty cells are represented by 0. The goal is to fill all cells with digits $1..9$ such that no digit repeats in any row, column, or $3 \times 3$ block.

```text
Algorithm SolveSudoku(grid):
    // 1. Find next empty cell
    If no empty cell exists:
        return TRUE  // Puzzle completely solved

    (row, col) = FindEmptyCell(grid)

    // 2. Try candidate digits 1 to 9
    For digit from 1 to 9:
        If IsSafe(grid, row, col, digit):
            grid[row][col] = digit  // Make tentative choice

            If SolveSudoku(grid) == TRUE:
                return TRUE  // Solution found

            // 3. BACKTRACKING STEP:
            grid[row][col] = 0      // Undo choice, restore state

    return FALSE  // Trigger backtrack to previous cell
```

```text
Function IsSafe(grid, row, col, digit):
    // Check row
    For c from 0 to 8:
        if grid[row][c] == digit: return FALSE
    // Check column
    For r from 0 to 8:
        if grid[r][col] == digit: return FALSE
    // Check 3x3 subgrid
    startRow = row - (row mod 3)
    startCol = col - (col mod 3)
    For r from 0 to 2:
        For c from 0 to 2:
            if grid[startRow + r][startCol + c] == digit: return FALSE
    return TRUE
```

#### 3. Explicit Description of the Backtracking Step [1 Mark]
* When `SolveSudoku` assigns `grid[row][col] = digit` and the subsequent recursive call returns `FALSE`, it means that no valid downstream assignment exists under this choice.
* The algorithm **unmakes the assignment** by executing:
  $$\text{grid}[\text{row}][\text{col}] = 0$$
* This resets the cell to its unassigned state and allows the loop to test `digit + 1`. If all digits $1..9$ fail, the function returns `FALSE`, causing the parent activation record on the call stack to undo its choice as well.

---

## QUESTION 6 [10 Marks]

### Q.6. (a) Directed Path Step vs. Jump Strategy (Randomized Optimal Stopping) [6 Marks]

We have a directed path with $n$ nodes: $1, 2, \dots, n$. Target is node $n$.
Moves from node $i$:
- **Step**: Moves to $i+1$ (cost 1 second).
- **Jump**: Roll $n$-sided die $\to$ moves to node $j \in \{1, \dots, n\}$ uniformly at random (cost 1 second).
Describe a strategy minimizing expected time to reach $n$. Prove the expected time is an asymptotic function of $n$.

#### 1. Mathematical Formulation & Structural Observation [2 Marks]
* Let $E(i)$ denote the expected remaining time to reach node $n$ starting from node $i$ under the optimal strategy.
* Boundary condition: $E(n) = 0$.
* If we choose to **Step** from node $i$, the expected remaining time is:
  $$\text{Cost}_{\text{step}}(i) = 1 + E(i + 1)$$
* If we choose to **Jump** from node $i$, the die lands on any node $j \in \{1, \dots, n\}$ with probability $\frac{1}{n}$. The expected remaining time is:
  $$\text{Cost}_{\text{jump}} = 1 + \frac{1}{n} \sum_{j=1}^n E(j) = J$$
  **Key Insight**: Notice that $\text{Cost}_{\text{jump}} = J$ is a **constant independent of the current position $i$**!
* By the Bellman Optimality Principle:
  $$E(i) = \min\big(1 + E(i+1), \, J\big)$$

#### 2. Threshold Policy Derivation [2 Marks]
* Because stepping takes us one step closer to $n$, $1 + E(i+1)$ decreases monotonically as $i$ approaches $n$.
* Near node $n$, deterministic stepping is faster than a random jump.
* Far from node $n$, taking random jumps is better because a jump can bypass many nodes in 1 second.
* Therefore, the optimal strategy is a **threshold policy** parametrized by an integer $k$:
  - For nodes $i \ge n - k$: **Step** (deterministic walk to $n$, since $n - i \le k$).
  - For nodes $i < n - k$: **Jump** (take a random jump).

* Under this threshold policy:
  - For $i \ge n - k$:
    $$E(n) = 0, \quad E(n-1) = 1, \quad E(n-2) = 2, \quad \dots, \quad E(n - r) = r \quad (\text{for } 0 \le r \le k)$$
  - For $i < n - k$:
    $$E(i) = J$$

#### 3. Solving for the Optimal Threshold $k$ & Expected Time [2 Marks]
* Express $J$ using the values of $E(j)$:
  $$J = 1 + \frac{1}{n} \left[ \sum_{j=1}^{n - k - 1} E(j) + \sum_{r=0}^k E(n - r) \right]$$
  There are $n - k - 1$ nodes that jump, each with expected time $J$.
  The $k+1$ nodes near $n$ have expected times $0, 1, 2, \dots, k$:
  $$\sum_{r=0}^k r = \frac{k(k + 1)}{2}$$
  Therefore:
  $$J = 1 + \frac{n - k - 1}{n} J + \frac{k(k + 1)}{2n}$$
* Multiply through by $n$:
  $$n J = n + (n - k - 1) J + \frac{k(k + 1)}{2}$$
  $$(k + 1) J = n + \frac{k(k + 1)}{2}$$
  $$J = \frac{n}{k + 1} + \frac{k}{2}$$

* Minimize $J(k)$ with respect to $k$:
  Treating $k$ as continuous and differentiating:
  $$\frac{d J}{d k} = -\frac{n}{(k + 1)^2} + \frac{1}{2} = 0 \iff (k + 1)^2 = 2n \iff k + 1 = \sqrt{2n}$$
  Thus, the optimal threshold is $k^* \approx \lfloor \sqrt{2n} \rfloor$.

* Calculate the optimal expected time $J^*$:
  Substitute $k + 1 = \sqrt{2n}$ into $J$:
  $$J^* = \frac{n}{\sqrt{2n}} + \frac{\sqrt{2n} - 1}{2} = \frac{\sqrt{2n}}{2} + \frac{\sqrt{2n}}{2} - \frac{1}{2} = \sqrt{2n} - \frac{1}{2}$$

* **Conclusion**:
  - **Optimal Strategy**:
    - At any node $i \in \{1, 2, \dots, n - \lfloor \sqrt{2n} \rfloor - 1\}$: **Jump** (roll die).
    - At any node $i \in \{n - \lfloor \sqrt{2n} \rfloor, \dots, n\}$: **Step** (move to $i+1$).
  - **Expected Time**: Starting from node 1, expected time is $J = \mathbf{\Theta(\sqrt{n})}$.

---

### Q.6. (b) Attempt any ONE [4 Marks]

#### OPTION (i): Fermat Numbers Coprimality Proof [4 Marks]
For integer $n \ge 0$, define Fermat numbers $f(n) = 2^{2^n} + 1$.
Prove that for distinct non-negative integers $m \ne n$, $\gcd(f(m), f(n)) = 1$.

* **Step 1: Prove by Induction the Product Identity**:
  $$f(n) = f(0) f(1) \dots f(n - 1) + 2 \quad (\text{for all } n \ge 1)$$

  - *Base Case ($n = 1$)*:
    $f(0) = 2^{2^0} + 1 = 2^1 + 1 = 3$.
    $f(1) = 2^{2^1} + 1 = 2^2 + 1 = 5$.
    $f(0) + 2 = 3 + 2 = 5 = f(1)$. The identity holds for $n = 1$.
  - *Inductive Hypothesis*: Assume true for $n = k$, i.e.:
    $$f(0) f(1) \dots f(k - 1) = f(k) - 2$$
  - *Inductive Step*: Multiply both sides by $f(k)$:
    $$f(0) f(1) \dots f(k - 1) f(k) = (f(k) - 2) f(k)$$
    Substitute $f(k) = 2^{2^k} + 1$:
    $$(f(k) - 2) f(k) = (2^{2^k} - 1)(2^{2^k} + 1) = (2^{2^k})^2 - 1^2 = 2^{2 \cdot 2^k} - 1 = 2^{2^{k+1}} - 1$$
    Notice that $f(k+1) = 2^{2^{k+1}} + 1$, which means $2^{2^{k+1}} - 1 = f(k+1) - 2$.
    Therefore:
    $$f(0) f(1) \dots f(k) = f(k+1) - 2 \iff f(k+1) = f(0) f(1) \dots f(k) + 2$$
    By mathematical induction, the identity holds for all $n \ge 1$.

* **Step 2: Prove $\gcd(f(m), f(n)) = 1$ for $m < n$**:
  - Let $d = \gcd(f(m), f(n))$. Then $d \mid f(m)$ and $d \mid f(n)$.
  - Since $m < n$, $f(m)$ is one of the factors in the product $P = f(0) f(1) \dots f(m) \dots f(n - 1)$.
  - Because $d \mid f(m)$, it follows that $d$ divides the entire product:
    $$d \mid \big(f(0) f(1) \dots f(n - 1)\big)$$
  - From the identity proved in Step 1:
    $$2 = f(n) - f(0) f(1) \dots f(n - 1)$$
  - Since $d$ divides both $f(n)$ and the product $f(0) \dots f(n-1)$, $d$ must divide their linear combination:
    $$d \mid 2$$
  - The only positive divisors of 2 are $1$ and $2$.
  - Now observe that for all $k \ge 0$:
    $$f(k) = 2^{2^k} + 1 = \text{Even} + 1 = \text{Odd}$$
  - Since $f(m)$ and $f(n)$ are both odd integers, their common divisor $d$ cannot be 2.
  - Therefore, $d = 1$, which proves:
    $$\mathbf{\gcd(f(m), f(n)) = 1} \quad \blacksquare$$

---

#### OPTION (ii): Full Binary Trees Recurrence & Catalan Number $f(7)$ [4 Marks]

A binary tree is full if every node has zero or two children. Let $f(n)$ denote the number of full binary trees with $n+1$ leaf nodes.

* **1. Recurrence Relation Derivation [2 Marks]**:
  - In a full binary tree with $n + 1$ leaves ($n \ge 1$), the root node has exactly two children: a left child and a right child.
  - Let $i$ be the number of leaf nodes in the left subtree.
  - Since the left subtree must have at least 1 leaf and at most $n$ leaves:
    $$1 \le i \le n$$
  - Consequently, the right subtree contains the remaining $(n + 1) - i$ leaves.
  - By definition:
    - The number of ways to form the left subtree with $i$ leaves is $f(i - 1)$.
    - The number of ways to form the right subtree with $(n + 1) - i$ leaves is $f((n + 1 - i) - 1) = f(n - i)$.
  - By the multiplication and addition rules of combinatorics:
    $$f(n) = \sum_{i=1}^n f(i - 1) \cdot f(n - i) \quad \text{for } n \ge 1$$
  - Base case: A full binary tree with 1 leaf node is just the single root node. Thus:
    $$f(0) = 1$$
  - This recurrence is the classical **Catalan Recurrence**:
    $$f(n) = C_n = \frac{1}{n + 1} \binom{2n}{n}$$

* **2. Computing $f(7)$ [2 Marks]**:
  - Using the recurrence or Catalan closed form:
    $$f(0) = 1$$
    $$f(1) = f(0)f(0) = 1 \times 1 = 1$$
    $$f(2) = f(0)f(1) + f(1)f(0) = 1 + 1 = 2$$
    $$f(3) = f(0)f(2) + f(1)f(1) + f(2)f(0) = 2 + 1 + 2 = 5$$
    $$f(4) = f(0)f(3) + f(1)f(2) + f(2)f(1) + f(3)f(0) = 5 + 2 + 2 + 5 = 14$$
    $$f(5) = f(0)f(4) + f(1)f(3) + f(2)f(2) + f(3)f(1) + f(4)f(0) = 14 + 5 + 4 + 5 + 14 = 42$$
    $$f(6) = f(0)f(5) + f(1)f(4) + f(2)f(3) + f(3)f(2) + f(4)f(1) + f(5)f(0) = 42 + 14 + 10 + 10 + 14 + 42 = 132$$
    $$f(7) = \frac{1}{7 + 1} \binom{14}{7} = \frac{1}{8} \cdot \frac{14 \times 13 \times 12 \times 11 \times 10 \times 9 \times 8}{7 \times 6 \times 5 \times 4 \times 3 \times 2 \times 1}$$
    $$f(7) = \frac{1}{8} \cdot 3432 = \mathbf{429}$$

* **Final Answer**:
  - Recurrence: $f(n) = \sum_{i=1}^n f(i - 1) f(n - i)$ with $f(0) = 1$.
  - Value: $\mathbf{f(7) = 429}$.

---
**End of Model Answers**
