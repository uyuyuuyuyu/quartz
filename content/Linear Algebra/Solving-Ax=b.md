# MIT 18.06SC Linear Algebra: Solving $Ax=b$

## 1. Solvability Conditions
* **Column Space Requirement:** The system $Ax=b$ has a solution only if $b$ is in the column space $C(A)$.
* **Row Combination Rule:** If a linear combination of the rows of $A$ results in a zero row, the same combination of the components of $b$ must also equal zero.
* **Elimination Method:** When using elimination on an augmented matrix, if a row of $A$ is completely eliminated, the corresponding entry in the transformed $b$ must also be zero.
* **Example Case:** In a matrix where the third row is the sum of the first and second, $Ax=b$ is only solvable if $b_3 = b_1 + b_2$.



---

## 2. The Complete Solution
To find all solutions, first verify solvability, then find a particular solution and add it to the nullspace.
$x_{complete} = x_p + x_n$ 

### Particular Solution ($x_p$)
* **Finding $x_p$:** Set all free variables to zero and solve for the remaining pivot variables.
* **Example:** For the matrix provided, setting $x_2 = x_4 = 0$ results in $x_p = \begin{bmatrix}-2 \\ 0 \\ 3/2 \\ 0\end{bmatrix}$.

### Nullspace Solution ($x_n$)
* **Definition:** The nullspace is the collection of all combinations of "special solutions" to the equation $Ax=0$.
* **Geometry:** The complete solutions to $Ax=b$ form a plane that is parallel to the nullspace but passes through $x_p$.



---

## 3. Rank ($r$) and Solution Outcomes
The rank ($r$) is the number of pivots in the matrix.

| Rank Condition | RREF Form ($R$) | Number of Solutions |
| :--- | :--- | :--- |
| **Full Rank** ($r=m=n$) | $I$  | Exactly 1  |
| **Full Column Rank** ($r=n < m$) | $\begin{bmatrix} I \\ 0 \end{bmatrix}$  | 0 or 1  |
| **Full Row Rank** ($r=m < n$) | $\begin{bmatrix} I & F \end{bmatrix}$  | Infinitely many  |
| **Partial Rank** ($r < m, r < n$) | $\begin{bmatrix} I & F \\ 0 & 0 \end{bmatrix}$  | 0 or infinitely many  |

### Summary of Key Cases
* **Full Column Rank ($r=n$):** The nullspace contains only the zero vector; there are no free variables or special solutions.
* **Full Row Rank ($r=m$):** The system is solvable for every $b$ because there are no zero rows in the reduced matrix to create constraints.