## Summary of the Four Fundamental Subspaces

Any $m \times n$ matrix $A$ determines four fundamental subspaces that describe the matrix's structure and its relationship to linear equations.



### Table: The Four Fundamental Subspaces
| Subspace | Notation | Dimension | Vector Space | Basis |
| :--- | :--- | :--- | :--- | :--- |
| **Column Space** | $C(A)$  | $r$ (rank)  | $\mathbb{R}^{m}$  | The $r$ pivot columns of $A$  |
| **Nullspace** | $N(A)$  | $n - r$  | $\mathbb{R}^{n}$  | Special solutions to $Ax = 0$  |
| **Row Space** | $C(A^{T})$  | $r$  | $\mathbb{R}^{n}$  | The first $r$ rows of the RREF matrix $R$  |
| **Left Nullspace** | $N(A^{T})$  | $m - r$  | $\mathbb{R}^{m}$  | The bottom $m - r$ rows of matrix $E$  |

---

### Key Properties and Concepts

#### 1. Characteristics of the Row Space
* The row space of a matrix $A$ is equivalent to the row space of its row-reduced echelon form, $R$.
* While row reduction changes the column space, it preserves the row space because the rows of $R$ are linear combinations of the rows of $A$.
* The first $r$ rows of $R$ constitute an "echelon" basis for the row space.

#### 2. Understanding the Left Nullspace
* The left nullspace consists of all vectors $y$ such that $A^{T}y = 0$, which can be rewritten as $y^{T}A = 0$.
* The term "left nullspace" is used because the vector $y^{T}$ appears on the left side of the matrix $A$ in the equation.
* To find a basis for this space, one reduces the augmented matrix $[A | I]$ to $[R | E]$, where $EA = R$.
* The bottom $m - r$ rows of $E$ correspond to the zero rows of $R$ and form the basis for the left nullspace.

---

### Matrices as Vector Spaces
Beyond standard vectors, the collection of all $3 \times 3$ matrices also forms a vector space, often called $M$. Within this space, matrices can be added and multiplied by scalars just like vectors. 

Specific subspaces of $M$ include:
* **Upper Triangular Matrices:** Matrices where all entries below the main diagonal are zero.
* **Symmetric Matrices:** Matrices that are equal to their own transpose.
* **Diagonal Matrices ($D$):** These matrices represent the intersection of the upper triangular and symmetric subspaces. For $3 \times 3$ matrices, the dimension of this subspace is 3.

---
Would you like me to walk through a numerical example of finding the matrix $E$ to determine the basis for a left nullspace?