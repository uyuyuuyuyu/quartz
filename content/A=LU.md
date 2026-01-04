# Linear Algebra: Factorization into $A=LU$

## 1. Fundamental Matrix Properties
* **Inverse of a Product:** The inverse of a matrix product $AB$ is $B^{-1}A^{-1}$.
* **Transpose of a Product:** The transpose of a matrix product $AB$ is $B^{T}A^{T}$.
* **Inverse of a Transpose:** For any invertible matrix $A$, the inverse of its transpose is the transpose of its inverse: $(A^{T})^{-1} = (A^{-1})^{T}$.

---

## 2. The $LU$ Decomposition Process
Gaussian elimination transforms a suitable matrix $A$ into an upper triangular matrix $U$. This leads to the factorization $A=LU$:
* **$U$ (Upper Triangular):** This matrix is upper triangular and contains the pivots on the diagonal.
* **$L$ (Lower Triangular):** This matrix is lower triangular and has ones on the diagonal.
* **$LDU$ Factorization:** Sometimes the pivots are factored out into a separate diagonal matrix $D$, resulting in the form $A=LDU$.

### Computational Complexity
* **Operation Count:** The total number of operations needed to factor an $n \times n$ matrix $A$ into $LU$ is on the order of $n^{3}$ (specifically $\approx \frac{1}{3}n^{3}$).
* **Efficiency:** Operating on the vector $b$ costs about $n^{2}$ operations, which is negligible compared to the matrix factorization.
* **Memory:** The computer can build $L$ "as we go" by keeping track of row subtractions, so it doesn't need to store the original matrix $A$ or the individual elimination matrices $E_{ij}$.

## $LDU$ Factorization
The $LDU$ factorization is an extension of $A=LU$ that explicitly separates the pivots into their own matrix.

### The Formula
$$A = LDU$$

### Matrix Definitions
* **$L$**: Lower triangular matrix with $1$s on the diagonal.
* **$D$**: Diagonal matrix containing the pivots ($d_1, d_2, ..., d_n$).
* **$U$**: Upper triangular matrix, scaled so that it has $1$s on the diagonal.

### Why use $LDU$?
It creates a symmetric representation. If the original matrix $A$ is symmetric ($A = A^T$), then $A = LDL^T$, which simplifies storage and computation significantly.

### Numerical Example ($2 \times 2$)
For $A = \begin{bmatrix} 2 & 1 \\ 8 & 7 \end{bmatrix}$:
1. **$L$ matrix**: $\begin{bmatrix} 1 & 0 \\ 4 & 1 \end{bmatrix}$ (using multiplier 4).
2. **$D$ matrix**: $\begin{bmatrix} 2 & 0 \\ 0 & 3 \end{bmatrix}$ (the pivots 2 and 3).
3. **$U$ matrix**: $\begin{bmatrix} 1 & 1/2 \\ 0 & 1 \end{bmatrix}$ (Row 1 divided by pivot 2).

---

## 3. Row Exchanges & Permutations
If a zero appears in a pivot position, row exchanges are required using a permutation matrix.
* **Permutation Matrix ($P$):** A matrix used to swap rows. For example, $P_{12}$ swaps the first and second rows.
* **Properties of $P$:** The inverse of any permutation matrix $P$ is its transpose: $P^{-1} = P^{T}$.
* **Group Theory:** There are $n!$ different permutation matrices for an $n \times n$ matrix, and they form a multiplicative group.

---

## 4. Numerical Example ($3 \times 3$)

### Starting Matrix $A$
$$A = \begin{bmatrix} 2 & 1 & 1 \\ 4 & 3 & 3 \\ 8 & 7 & 9 \end{bmatrix}$$

### Step 1: Elimination to find $U$
1. **Subtract $2 \times$ (Row 1) from Row 2:**
   $$\begin{bmatrix} 2 & 1 & 1 \\ 0 & 1 & 1 \\ 8 & 7 & 9 \end{bmatrix}$$
2. **Subtract $4 \times$ (Row 1) from Row 3:**
   $$\begin{bmatrix} 2 & 1 & 1 \\ 0 & 1 & 1 \\ 0 & 3 & 5 \end{bmatrix}$$
3. **Subtract $3 \times$ (Row 2) from Row 3:**
   $$U = \begin{bmatrix} 2 & 1 & 1 \\ 0 & 1 & 1 \\ 0 & 0 & 2 \end{bmatrix}$$

### Step 2: Constructing $L$
The multipliers used were **2**, **4**, and **3**. We place these in $L$ at positions $(2,1)$, $(3,1)$, and $(3,2)$ respectively, with $1$s on the diagonal:
$$L = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 4 & 3 & 1 \end{bmatrix}$$

### Final Verification
$$A = LU \implies \begin{bmatrix} 2 & 1 & 1 \\ 4 & 3 & 3 \\ 8 & 7 & 9 \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 \\ 2 & 1 & 0 \\ 4 & 3 & 1 \end{bmatrix} \begin{bmatrix} 2 & 1 & 1 \\ 0 & 1 & 1 \\ 0 & 0 & 2 \end{bmatrix}$$