# 18.06SC Session 1.6: Column Space and Nullspace

## Review of Subspaces
* **Vector Space**: A collection of vectors closed under linear combinations. For any vectors $v, w$ and real numbers $c, d$, the vector $cv + dw$ is also in the space.
* **Subspace**: A vector space contained inside another vector space.
    * Examples in $\mathbb{R}^3$: A plane or a line that contains the origin $\begin{bmatrix}0\\ 0\\ 0\end{bmatrix}$.
* **Intersections and Unions**:
    * The intersection $S \cap T$ of two subspaces is a subspace.
    * The union $P \cup L$ of two subspaces is generally **not** a subspace because the sum of vectors from each may fall outside the union.

---

## Column Space of A ($C(A)$)
* **Definition**: The vector space made up of all linear combinations of the columns of $A$.
* **Solving $Ax = b$**:
    * The system $Ax = b$ is solvable exactly when $b$ is a vector in the column space of $A$.
    * If $x$ is a solution, $b$ must be a linear combination of the columns of $A$.
* **Example Matrix**:
    $$A = \begin{bmatrix} 1 & 1 & 2 \\ 2 & 1 & 3 \\ 3 & 1 & 4 \\ 4 & 1 & 5 \end{bmatrix}$$
    * The third column is the sum of the first two ($col_1 + col_2 = col_3$), so it adds no new information to the subspace.
    * The column space for this matrix is a **two-dimensional** subspace of $\mathbb{R}^4$.

---

## Nullspace of A ($N(A)$)
* **Definition**: The collection of all solutions to the equation $Ax = 0$.
* **Vector Space Property**: It is a subspace because any sum or multiple of solutions to $Ax = 0$ is also a solution ($A(x_1 + x_2) = 0$ and $A(cx) = 0$).
* **Example Nullspace**:
    * For the matrix above, the nullspace consists of all multiples of $\begin{bmatrix} 1 \\ 1 \\ -1 \end{bmatrix}$.
    * This is because $column_1 + column_2 - column_3 = 0$.
    * This nullspace is a line in $\mathbb{R}^3$.

---

## Solutions to $Ax = b$ (where $b \neq 0$)
* Solutions to $Ax = b$ for a non-zero $b$ **do not** form a subspace.
* **Reason**: The zero vector is not a solution to the equation.
* **Example**: For $b = \begin{bmatrix} 1 & 2 & 3 & 4 \end{bmatrix}^T$, the solutions form a line in $\mathbb{R}^3$ that does not pass through the origin.