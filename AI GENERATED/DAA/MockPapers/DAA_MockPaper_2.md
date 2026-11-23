# End-Semester Examination: Design and Analysis of Algorithms (CS 3205 / CS 3212)
## Case-Based Examination Paper (Pattern AY 2026-27)

**Vishwakarma Institute of Technology, Pune - 411037**  
*(An Autonomous Institute Affiliated to Savitribai Phule Pune University)*  

**Examination**: End Semester Examination (ESE) | **Year**: TY | **Branch**: Computer Engineering  
**Subject**: Design and Analysis of Algorithms | **Subject Code**: CS 3205 / CS 3212  
**Max. Marks**: 60 | **Duration**: 2 Hours (Time: 3:00 PM – 5:00 PM) | **Pages**: 3  

---

### Instructions to Candidate:
1. Figures to the right indicate full marks.
2. Try to avoid using informal arguments. You will not get any credit for them. Try to write formal, concrete, and concise answers.
3. Use simple English language along with simple programming constructs while describing the algorithms. If the examiner cannot understand the programming constructs or the syntax you used, then naturally you will not get any credit. Your goal is to convey your algorithm to the examiner in the simplest possible language (of course without compromising concreteness, correctness).
4. While describing algorithms you must use only simple data structures like arrays, linked list, trees, etc. Any use of any advanced data structures like sets, hashmap, etc without giving implementation details carry no weightage.

---

## Q.1. Attempt the Following. [10 Marks]

### Q.1. (a) [5 Marks] [CO: 3, BT: 2, 3]
Consider following pair of functions $f(n)$ and $g(n)$. You need to choose correct options amongst $f(n) = o(g(n))$ or $f(n) = \Theta(g(n))$ or $f(n) = \Omega(g(n))$, where $o, \Theta, \Omega$ are small-oh, Big-Theta, Big-Omega asymptotic notations. Give formal justification for your answer using limits or bounding constants.  
(i) $f(n) = (\log_2 n)^{\log_2 n}, \quad g(n) = \frac{n}{\log_2 n}$ [2.5M]  
(ii) $f(n) = 3^{\sqrt{\log_2 n}}, \quad g(n) = n^{1/3}$ [2.5M]  

### Q.1. (b) [5 Marks] [CO: 2, 3, 6, BT: 3, 4]
Suppose you are given a tree $T = (V, E)$ with $n$ vertices, where each vertex $v \in V$ is assigned a strictly positive integer weight $w(v) > 0$. We wish to choose a subset of vertices $S \subseteq V$ such that no two chosen vertices are adjacent in $T$, and the sum of weights $\sum_{v \in S} w(v)$ is maximized.  
Design an efficient algorithm for this task using simple data structures. What is the time and space complexity of your algorithm? For full credit, time complexity of your algorithm must be asymptotically equal to the time complexity of the best algorithm for this task ($\Theta(n)$). Prove its correctness and asymptotic efficiency.

---

## Q.2. Attempt the Following. [12 Marks]

### Q.2. (a) [6 Marks] [CO: 1, 2, 3, BT: 3, 4]
Given a natural number $n$, we wish to compute the $n$-th Fibonacci number $F_n \bmod m$, where $F_0 = 0, F_1 = 1$, and $F_n = F_{n-1} + F_{n-2}$ for $n \ge 2$.  
(i) Formulate the state transition using a $2 \times 2$ matrix and describe a recursive divide-and-conquer algorithm to compute $F_n \bmod m$. The time complexity of your algorithm must be logarithmic in $n$ ($O(\log n)$ integer arithmetic operations). [3M]  
(ii) Using your algorithm, trace the computation of $F_{13} \bmod 1000$ step-by-step showing each matrix multiplication and modulo reduction. [3M]  

### Q.2. (b) [6 Marks] [CO: 1, 2, 3, BT: 3, 4]
Consider a variation of the Josephus game with $n$ players numbered $1, 2, \dots, n$ standing in a circle. Player 1 holds the elimination sword. Instead of killing the immediate neighbor, the game follows a **skip-two elimination rule**: the player holding the sword skips the next player, eliminates the second player in clockwise order, and hands the sword to the next alive player.  
Let $W(n)$ denote the position of the survivor among $n$ players.  
(i) Formulate a clean recurrence relation for $W(n)$ for even and odd circle sizes, and provide a formal proof of correctness for your recurrence. [3M]  
(ii) Using your recurrence, determine the winner for:  
    1. $n = 8$ players  
    2. $n = 16$ players  
    State the general pattern for $n = 2^m$ players under this elimination rule. [3M]  

---

## Q.3. Attempt the Following. [8 Marks]

### Q.3. (a) Attempt any ONE of the following. [5 Marks] [CO: 1, 2, 3, BT: 2, 3]

#### OPTION (a)(i)
Let $G = (V, E)$ be a directed acyclic graph (DAG) where each directed edge $(u, v) \in E$ has a real-valued weight $w(u, v)$ (weights may be negative or positive). For vertices $s, t \in V$, we wish to find the **longest simple path** (path of maximum total edge weight) from $s$ to $t$.  
(1) Give a concrete recurrence relation for $L(v)$, the weight of the longest path from source $s$ to vertex $v$. Describe the base cases.  
(2) Using the recurrence and topological sorting, design an efficient dynamic programming algorithm using simple arrays to compute the longest path from $s$ to $t$.  
(3) State the exact time and space complexity of your algorithm. Explain why this problem is efficiently solvable on DAGs in polynomial time, whereas on general directed graphs with cycles it is NP-hard. [5M]  

**OR**

#### OPTION (a)(ii)
Consider a text over alphabet $\{A, B, C, D, E, F, G\}$ with relative frequencies $4, 6, 11, 13, 17, 21, 28$ respectively.  
(1) Construct the complete Huffman coding tree step-by-step, showing intermediate forest states.  
(2) Determine the optimal binary prefix code assigned to each of the seven characters.  
(3) Suppose a document contains total $50,000$ characters with these exact relative frequencies. Calculate the total size of the encoded text in bits, and compute the average bits per character. [5M]  

---

### Q.3. (b) Attempt any ONE of the following. [3 Marks] [CO: 3, BT: 1, 4]

#### OPTION (b)(i)
For the following recurrence, use the Master's Theorem to give the best possible asymptotic bound:  
$$T(n) = 8 T(n/2) + n^3 \log_2 n$$  
Give formal mathematical justification. Express your answer using Big-Theta ($\Theta$) notation. [3M]  

**OR**

#### OPTION (b)(ii)
Prove that $\sum_{k=1}^n k \log_2 k = \Theta(n^2 \log_2 n)$ using integral bounding or by splitting the summation into halves. Provide formal bounding constants. [3M]  

---

## Q.4. Attempt the Following. [10 Marks]

### Q.4. (a) [5 Marks] [CO: 1, 3, 6, BT: 3, 4, 5]
On a circular running track of length $L$, there are $2n$ distinct checkpoint markers: $n$ markers are painted **Red** and $n$ markers are painted **Blue**.  
Prove that there exists a straight diameter line cutting the circular track into two semicircles of equal arc length such that each semicircle contains exactly $n/2$ Red markers and $n/2$ Blue markers (assume $n$ is an even natural number, and no two markers are diametrically opposite).  
*(Note: For full credit, you must give a concrete mathematical proof using a discrete or continuous invariant argument. Vague or hand-waving arguments carry no weightage).* [5M]  

### Q.4. (b) [5 Marks] [CO: 2, 3, 6, BT: 3, 4, 5]
Given two text strings $X = x[1..m]$ and $Y = y[1..n]$, we define the **Damerau-Levenshtein Edit Distance** as the minimum cost to transform $X$ into $Y$ using four primitive operations:
- **Insertion** of a character (cost 1)
- **Deletion** of a character (cost 1)
- **Substitution** of a character (cost 1 if characters differ, 0 if identical)
- **Adjacent Transposition**: Swapping two adjacent characters $x[i] x[i-1] \leftrightarrow x[i-1] x[i]$ (cost 1).
Let $D(i, j)$ denote the minimum edit distance between prefixes $X[1..i]$ and $Y[1..j]$.  
(1) Formulate a clean, concrete recurrence relation for $D(i, j)$ accounting for all four operations.  
(2) Describe the base cases of the recurrence.  
(3) Design an efficient dynamic programming algorithm using a 2D array, and state its time and space complexity. [5M]  

---

## Q.5. Attempt the Following. [10 Marks]

### Q.5. (a) [5 Marks] [CO: 1, 2, 4, BT: 1, 2, 3]
Precisely define the decision problems **3-SAT** and **INDEPENDENT SET** (for both problems, explicitly specify the input, what constitutes a YES instance, and what constitutes a NO instance).  
Provide a polynomial-time reduction from **3-SAT to INDEPENDENT SET** ($3\text{-SAT} \le_P \text{INDEPENDENT SET}$). Prove the bidirectional correctness of your reduction. [5M]  

### Q.5. (b) [5 Marks] [CO: 1, 2, 4, BT: 1, 2]
What is the state-space tree in the backtracking algorithmic paradigm?  
Formulate the **Knight's Tour Problem** on an $N \times N$ chessboard as a backtracking search problem (a knight must visit every square of the chessboard exactly once).  
(1) Describe the state representation and choice vector.  
(2) Define the validity / bounding function `isSafe()`.  
(3) Present the complete recursive backtracking algorithm and clearly identify the backtracking step. [5M]  

---

## Q.6. Attempt the Following. [10 Marks]

### Q.6. (a) [6 Marks] [CO: 2, 3, 5, 6, BT: 3, 6]
Consider a game played on a circular ring of $n$ cells numbered $0, 1, 2, \dots, n-1$ in clockwise order. We start at cell 0, and our objective is to reach the target cell $n-1$ as quickly as possible.  
At each second, standing at cell $i$, we must choose one of the following two actions:
- **Walk**: Move deterministically to the next cell $(i + 1) \bmod n$. (Takes 1 second).
- **Teleport**: Flip an unbiased fair coin. If Heads (probability $1/2$), teleport directly to the target cell $n-1$. If Tails (probability $1/2$), stay at the current cell $i$. (Takes 1 second).

Describe a strategy that minimizes the expected time to reach cell $n-1$. What is the expected time taken by your strategy as an asymptotic function of $n$? Give a concrete mathematical proof that the expected time of your strategy is as claimed. [6M]  

---

### Q.6. (b) Attempt any ONE of the following. [4 Marks]

#### OPTION (b)(i) [CO: 1, 3, BT: 1, 3]
State **Lamé's Theorem** regarding the number of division steps executed by the Euclidean Algorithm for computing $\gcd(a, b)$.  
Prove that if $a > b \ge 1$ and the Euclidean Algorithm requires $k$ division steps, then $b \ge F_{k+1}$ (where $F_{k+1}$ is the $(k+1)$-th Fibonacci number with $F_1 = 1, F_2 = 1, F_3 = 2$). Using this inequality, formally prove that the worst-case time complexity of the Euclidean Algorithm is $O(\log b)$. [4M]  

**OR**

#### OPTION (b)(ii) [CO: 1, 3, BT: 2, 3]
A convex polygon with $n + 2$ vertices ($n \ge 1$) can be divided into $n$ non-overlapping triangles by drawing $n - 1$ non-intersecting internal diagonals. Let $T(n)$ denote the number of distinct ways to triangulate such a convex polygon.  
(1) Formulate a recurrence relation for $T(n)$ by fixing an arbitrary base edge of the polygon.  
(2) Identify the combinatorial sequence represented by $T(n)$ and state its closed-form formula.  
(3) Calculate the exact number of triangulations for a convex heptagon (7 vertices, $n = 5$). [4M]  

---
**End of Question Paper**
