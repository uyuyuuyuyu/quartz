# Lecture Notes: Matrix Spaces, Rank, and Small World Graphs

## 1. Matrix Spaces
We can extend the concept of vector spaces beyond $\mathbb{R}^n$ to any set of "vectors" (like matrices) that allow for addition and scalar multiplication.

### The Space of $3 \times 3$ Matrices ($M$)
* **Definition:** The space $M$ contains all $3 \times 3$ matrices.
* **Dimension:** The dimension of $M$ is 9 because 9 numbers are required to specify an element (similar to $\mathbb{R}^9$).

### Subspaces of $M$
We identified three specific subspaces within $M$:
1.  **Symmetric Matrices ($S$):**
    * **Dimension:** 6. We choose 3 numbers for the diagonal and 3 for the upper right; the lower left is determined by symmetry.
2.  **Upper Triangular Matrices ($U$):**
    * **Dimension:** 6. Similar freedom of choice as symmetric matrices (3 diagonal, 3 upper right).
3.  **Diagonal Matrices ($D$):**
    * **Definition:** The intersection of symmetric and upper triangular matrices ($D = S \cap U$).
    * **Dimension:** 3. Only the diagonal entries are free to be chosen.
    * **Basis:** The basis for $D$ is the intersection of the bases for $S$ and $U$.

### Unions vs. Sums
* **Union ($S \cup U$):** The set of matrices that are *either* symmetric *or* upper triangular is **not** a subspace. It is analogous to two lines in $\mathbb{R}^2$ which do not form a subspace together.
* **Sum ($S + U$):** The set of all possible sums of elements from $S$ and $U$ is a valid subspace. In this case, $S + U = M$.

### Dimensional Formula
For sums and intersections of subspaces, the dimensions follow this rule:
$$\dim(S) + \dim(U) = \dim(S+U) + \dim(S \cap U)$$
*Substituting the values from this lecture:*
$$6 + 6 = 9 + 3$$

---

## 2. Vector Spaces in Differential Equations
Vector spaces also appear in differential equations, where solutions acts as vectors.
* **Example:** Solutions to the differential equation $\frac{d^{2}y}{dx^{2}}+y=0$.
* **Vector Space Structure:** The solutions form a space similar to a nullspace.
* **Basis:** The space is 2-dimensional with basis vectors $\cos x$ and $\sin x$.
* **General Solution:** $y = c_{1}\cos x + c_{2}\sin x$.

---

## 3. Rank and Subspaces

### Rank 4 Matrices (Not a Subspace)
In the space of $5 \times 17$ matrices, the set of all Rank 4 matrices is **not** a subspace.
* **Reason:** It is not closed under addition. The sum of two Rank 4 matrices might not have Rank 4 (e.g., the sum could be Rank 5).

### Example of a Valid Subspace in $\mathbb{R}^4$
Consider the set of vectors $\mathbf{v}$ where components sum to zero: $v_1+v_2+v_3+v_4=0$.
* **Nullspace Connection:** This set is the nullspace of the matrix $A = [\begin{matrix}1 & 1 & 1 & 1\end{matrix}]$.
* **Dimension:** Since $A$ has Rank $r=1$ and $n=4$, the dimension is $n-r = 3$.
* **Basis:** The subspace is spanned by three special solutions:
    $$\begin{bmatrix}-1\\1\\0\\0\end{bmatrix}, \begin{bmatrix}-1\\0\\1\\0\end{bmatrix}, \begin{bmatrix}-1\\0\\0\\1\end{bmatrix}$$

### Rank 1 Matrices
* **Definition:** A matrix has Rank 1 if its column/row space has dimension 1.
* **Structure:** Every Rank 1 matrix can be written as $A = UV^T$, where $U$ and $V$ are column vectors.
* **Usage:** They are used as "building blocks" for more complex matrices.

---

## 4. Small World Graphs
Matrices are useful for describing graphs to solve problems regarding connectivity and distance.
* **Graph Definition:** A collection of nodes joined by edges ($G = \{\text{nodes, edges}\}$).
* **Examples:**
    * **Social Graphs:** Nodes are people; edges are friendships.
    * **World Wide Web:** Nodes are websites; edges are links.
* **Distance:** We use these graphs to calculate the "degrees of separation" (shortest path) between nodes.