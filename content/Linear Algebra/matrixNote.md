# Lecture 3: Multiplication and Inverse Matrices

## Matrix Multiplication
If A is an $m \times n$ matrix and B is an $n \times p$ matrix, the resulting matrix C is an $m \times p$ matrix. The entry in row $i$ and column $j$ of matrix C is denoted as $c_{ij}$.

### Four Ways to Think About $AB = C$
* **Standard (Row times Column):** $c_{ij}$ equals the dot product of row $i$ of matrix A and column $j$ of matrix B.
    * Formula: $c_{ij}=\sum_{k=1}^{n}a_{ik}b_{kj}$.
* **Columns:** Column $j$ of matrix C is the product of matrix A and column $j$ of matrix B, meaning the columns of C are combinations of columns of A.
* **Rows:** Row $i$ of matrix C is the product of row $i$ of matrix A and matrix B, meaning the rows of C are combinations of rows of B.
* **Column times Row:** The product is the sum of matrices formed by multiplying a single column of A ($m \times 1$) by a single row of B ($1 \times p$).
    * Formula: $AB=\sum_{k=1}^{n}[\text{column } k \text{ of } A][\text{row } k \text{ of } B]$.
    * Each "column times row" matrix has a row space and column space that is a single line.



### Blocks
If matrices A and B are subdivided into blocks that match properly, the product $AB=C$ can be written in terms of block products.
* Example: $\begin{bmatrix} A_1 & A_2 \\ A_3 & A_4 \end{bmatrix} \begin{bmatrix} B_1 & B_2 \\ B_3 & B_4 \end{bmatrix} = \begin{bmatrix} C_1 & C_2 \\ C_3 & C_4 \end{bmatrix}$ where $C_1 = A_1B_1 + A_2B_3$.

Markdown

## Block Multiplication: 3x3 Numerical Example

[cite_start]If matrices $A$ and $B$ are subdivided into blocks that match properly, we can write the product $AB=C$ in terms of products of the blocks[cite: 22]. [cite_start]Treating these blocks as individual entries allows for a more organized calculation[cite: 23].



### 1. Setup the Matrices
Let’s partition $3 \times 3$ matrices $A$ and $B$ into a $2 \times 2$ block, a $2 \times 1$ block, a $1 \times 2$ block, and a $1 \times 1$ block:

$$A = \begin{bmatrix} 1 & 0 & \mid & 2 \\ 0 & 1 & \mid & 3 \\ \hline 4 & 5 & \mid & 6 \end{bmatrix}, \quad B = \begin{bmatrix} 1 & 2 & \mid & 0 \\ 3 & 4 & \mid & 0 \\ \hline 0 & 0 & \mid & 1 \end{bmatrix}$$

**Sub-blocks defined:**
* $A_1 = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix}, A_2 = \begin{bmatrix} 2 \\ 3 \end{bmatrix}, A_3 = \begin{bmatrix} 4 & 5 \end{bmatrix}, A_4 = [6]$
* $B_1 = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}, B_2 = \begin{bmatrix} 0 \\ 0 \end{bmatrix}, B_3 = [0 \quad 0], B_4 = [1]$

### 2. Calculate the Resulting Blocks
[cite_start]The product $C$ is found using the formula $C_1 = A_1B_1 + A_2B_3$[cite: 24].

* **Top-Left Block ($C_1$):**
  $$C_1 = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix} \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} + \begin{bmatrix} 2 \\ 3 \end{bmatrix} \begin{bmatrix} 0 & 0 \end{bmatrix} = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} + \begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix} = \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$$

* **Top-Right Block ($C_2 = A_1B_2 + A_2B_4$):**
  $$C_2 = \begin{bmatrix} 1 & 0 \\ 0 & 1 \end{bmatrix} \begin{bmatrix} 0 \\ 0 \end{bmatrix} + \begin{bmatrix} 2 \\ 3 \end{bmatrix} [1] = \begin{bmatrix} 0 \\ 0 \end{bmatrix} + \begin{bmatrix} 2 \\ 3 \end{bmatrix} = \begin{bmatrix} 2 \\ 3 \end{bmatrix}$$

* **Bottom-Left Block ($C_3 = A_3B_1 + A_4B_3$):**
  $$C_3 = \begin{bmatrix} 4 & 5 \end{bmatrix} \begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix} + [6][0 \quad 0] = [4(1)+5(3) \quad 4(2)+5(4)] = [19 \quad 28]$$

* **Bottom-Right Block ($C_4 = A_3B_2 + A_4B_4$):**
  $$C_4 = \begin{bmatrix} 4 & 5 \end{bmatrix} \begin{bmatrix} 0 \\ 0 \end{bmatrix} + [6][1] = 6$$

### 3. Final Matrix Result
Assembling the blocks back into a $3 \times 3$ matrix:

$$C = \begin{bmatrix} 1 & 2 & 2 \\ 3 & 4 & 3 \\ 19 & 28 & 6 \end{bmatrix}$$

---

## Inverses
A square matrix A is invertible (or nonsingular) if there exists an inverse $A^{-1}$ such that $A^{-1}A = I = AA^{-1}$.

### Singular Matrices
A matrix A is singular (not invertible) if:
* Its determinant is zero.
* There exists a non-zero vector $x$ such that $Ax = 0$.
    * Example: $\begin{bmatrix} 1 & 3 \\ 2 & 6 \end{bmatrix} \begin{bmatrix} 3 \\ -1 \end{bmatrix} = \begin{bmatrix} 0 \\ 0 \end{bmatrix}$. In this case, the columns lie on the same line.

---

## Gauss-Jordan Elimination
This method uses row elimination to solve for the inverse of a matrix.



### The Procedure
1. **Augment the matrix:** Place the identity matrix next to A: $[A | I]$.
2. **Elimination:** Use Gaussian elimination to transform A into upper triangular form.
3. **Jordan Method:** Eliminate entries in the upper right portion of the matrix to reach $[I | A^{-1}]$.

### Matrix Representation
If E is the product of all elimination matrices used, the process is $E[A|I] = [I|E]$. Since $EA=I$, then $E = A^{-1}$.


# Exercises on Column Space and Nullspace

## Problem 6.1 (Strang 3.1 #30)
Suppose $S$ and $T$ are two subspaces of a vector space $V$.

### a) Definition of Sum
The sum $S + T$ contains all sums $s + t$ of a vector $s$ in $S$ and a vector $t$ in $T$. Show that $S + T$ satisfies the requirements (addition and scalar multiplication) for a vector space.

**Solution:**
Let $s, s'$ be vectors in $S$, let $t, t'$ be vectors in $T$, and let $c$ be a scalar. Then:
* **Addition**: $(s + t) + (s' + t') = (s + s') + (t + t')$.
* **Scalar Multiplication**: $c(s + t) = cs + ct$.

Thus $S + T$ is closed under addition and scalar multiplication; in other words, it satisfies the two requirements for a vector space.

### b) Sum vs. Union
If $S$ and $T$ are lines in $\mathbb{R}^m$, what is the difference between $S + T$ and $S \cup T$? That union contains all vectors from $S$ and $T$ or both. Explain this statement: *The span of $S \cup T$ is $S + T$*.



**Solution:**
If $S$ and $T$ are distinct lines, then $S + T$ is a plane, whereas $S \cup T$ is only the two lines. The span of $S \cup T$ is the set of all combinations of vectors in this union of two lines. In particular, it contains all sums $s + t$ of a vector $s$ in $S$ and a vector $t$ in $T$, and these sums form $S + T$.

Since $S + T$ contains both $S$ and $T$, it contains $S \cup T$. Further, $S + T$ is a vector space, so it contains all combinations of vectors in itself; in particular, it contains the span of $S \cup T$. Thus the span of $S \cup T$ is $S + T$.

---

## Problem 6.3 (Strang 3.2 #36)
How is the nullspace $N(C)$ related to the spaces $N(A)$ and $N(B)$, if $C = \begin{bmatrix} A \\ B \end{bmatrix}$?

**Solution:**
$N(C) = N(A) \cap N(B)$ contains all vectors that are in both nullspaces:
$$Cx = \begin{bmatrix} Ax \\ Bx \end{bmatrix} = 0$$
if and only if $Ax = 0$ and $Bx = 0$.