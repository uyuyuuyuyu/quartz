# Independence, Basis, and Dimension

This summary outlines how independence, spanning, and basis are used to describe vector subspaces like the nullspace and column space.

---

## 1. Linear Independence
* **Definition**: Vectors $x_{1}, x_{2}, \dots, x_{n}$ are linearly independent if $c_{1}x_{1} + c_{2}x_{2} + \dots + c_{n}x_{n} = 0$ only when all coefficients $c_{1}, \dots, c_{n}$ are 0.
* **Geometric View**: Two vectors are independent if they do not lie on the same line; three vectors are independent if they do not lie in the same plane.
* **Matrix Context**: The columns of matrix $A$ are independent exactly when the nullspace $N(A)$ contains only the zero vector.
* **Free Variables**: If columns are independent, there are no free variables and the rank equals $n$.
* **Dependency Rule**: If a matrix has more unknowns than equations ($m < n$), the columns are dependent because there is at least one free variable.

---

## 2. Spanning a Space
* **Definition**: Vectors span a space when the space consists of all possible linear combinations of those vectors.
* **Column Space**: The column vectors of $A$ span the column space $C(A)$.
* **Minimal Space**: A spanning set defines the smallest space containing those specific vectors.

---

## 3. Basis and Dimension
* **Definition of Basis**: A basis is a sequence of vectors that are both independent and span the vector space.
* **Unique Property**: Every basis for a specific space has the same number of vectors.
* **Dimension**: The number of vectors in a basis is called the dimension of the space.
* **R^n Basis**: Any $n$ vectors in $\mathbb{R}^{n}$ form a basis if they are the columns of an invertible matrix.

---

## 4. Bases of Column Space and Nullspace
* **Column Space $C(A)$**: 
    * The pivot columns of $A$ form a basis for $C(A)$.
    * Dimension of $C(A) = \text{rank}(A) = \text{number of pivot columns}$.
* **Nullspace $N(A)$**: 
    * The "special solutions" to $Ax = 0$ form a basis for the nullspace.
    * Dimension of $N(A) = \text{number of free variables} = n - r$.
* **Terminology Note**: Matrices have a **rank**; subspaces have a **dimension**.

---

## 5. Worked Example
For matrix $A = \begin{bmatrix} 1 & 2 & 3 & 1 \\ 1 & 1 & 2 & 1 \\ 1 & 2 & 3 & 1 \end{bmatrix}$:
* **Rank**: 2.
* **Pivot Columns**: Columns 1 and 2 form the basis for $C(A)$.
* **Nullspace Basis**: Since there are 2 free variables ($4 - 2 = 2$), the dimension is 2.
* **Special Solutions**: The vectors $\begin{bmatrix} -1 \\ -1 \\ 1 \\ 0 \end{bmatrix}$ and $\begin{bmatrix} -1 \\ 0 \\ 0 \\ -1 \end{bmatrix}$ form the basis for $N(A)$.