# Solving $Ax = 0$: Pivot Variables and Special Solutions

## 1. Computing the Nullspace
The nullspace of a matrix $A$ consists of all vectors $x$ that satisfy the equation $Ax = 0$. We use elimination to find these solutions, even for matrices that are not invertible.

* **Row Operations:** These operations do not change the nullspace (the solutions to $Ax = 0$), although they do change the column space.
* **No Augmented Matrix:** We do not need to use an augmented matrix $[A|0]$ because the right side remains zero throughout the computation.
* **Echelon Form ($U$):** The first goal of elimination is to reach a staircase form called echelon form.

## 2. Rank and Variables
* **Rank ($r$):** The rank of a matrix is equal to the number of pivots it contains.
* **Pivot Variables:** These correspond to columns that contain a pivot.
* **Free Variables:** These correspond to columns that do not have a pivot. You can assign any value to these variables.
* **Dimension:** The number of free variables is $n - r$ (total columns minus the rank), which is the dimension of the nullspace.

## 3. Special Solutions
Special solutions are specific vectors that span the nullspace.

* **Method:** To find a special solution, set one free variable to $1$ and all other free variables to $0$. Use back-substitution to solve for the pivot variables.
* **Complete Solution:** The nullspace is the collection of all linear combinations of these special solution vectors.

## 4. Reduced Row Echelon Form (RREF)
By continuing elimination, we convert $U$ into $R$ (Reduced Row Echelon Form).
* **Characteristics of $R$:** * Pivots are equal to $1$.
    * There are zeros above and below every pivot.
* **Block Structure:** If the columns are reordered so pivots come first, $R$ can be written as:
  $$R = \begin{bmatrix} I & F \\ 0 & 0 \end{bmatrix}$$
  Where $I$ is the $r \times r$ identity matrix and $F$ is the $r \times (n-r)$ matrix of free columns.

## 5. The Nullspace Matrix ($N$)
The nullspace matrix $N$ is a matrix whose columns are the special solutions. It can be constructed directly from $R$:
$$N = \begin{bmatrix} -F \\ I \end{bmatrix}$$

### Proof of $RN = 0$
Using block multiplication:
$$RN = \begin{bmatrix} I & F \end{bmatrix} \begin{bmatrix} -F \\ I \end{bmatrix} = (I \times -F) + (F \times I) = -F + F = 0$$
This confirms that the columns of $N$ are indeed the solutions to $Ax = 0$.