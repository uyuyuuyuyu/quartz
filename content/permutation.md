# MIT 18.06SC Linear Algebra: Transposes, Permutations, Spaces $\mathbb{R}^n$

## 1. Permutations and Transposes
-  **Permutations**: Multiplication by a permutation matrix $P$ swaps the rows of a matrix  .  These are used during elimination to move zeros out of pivot positions  .
-  **Factorization**: The standard $A = LU$ becomes $PA = LU$ when row reordering is necessary  .
-  **Properties**: Permutation matrices satisfy $P^{-1} = P^T$, meaning $P^T P = I$  .
-  **Transposes**: Taking a transpose turns rows into columns and columns into rows  .  Formally, $(A^T)_{ij} = A_{ji}$  .
-  **Symmetry**: A matrix is symmetric if $A^T = A$  .
-  **Symmetric Products**: For any matrix $R$, the product $R^T R$ is always symmetric because $(R^T R)^T = R^T (R^T)^T = R^T R$  .

## 2. Vector Spaces and Closure
-  **Definition**: A vector space is a collection where you can add vectors and multiply them by scalars to form linear combinations  14, 15].
-  **$\mathbb{R}^n$**: The set of all column vectors with $n$ real number components  .
-  **Closure Rules**: To qualify as a space, the set must be closed under addition (sum stays in the set) and scalar multiplication (product with any real number stays in the set)  .
-  **Non-example**: A set of only positive vectors is not a vector space  .  While closed under addition, multiplying by a negative scalar results in a vector outside the set  21, 23].

## 3. Subspaces
-  **Definition**: A vector space contained inside another vector space  .
-  **Zero Vector**: Every subspace **must** contain the zero vector because spaces are closed under multiplication (e.g., $0 \cdot v = 0$)  31, 32].
- **Subspaces of $\mathbb{R}^2$**:
  1.  All of $\mathbb{R}^2$  .
  2.  Any line through the origin  35, 36].
  3.  The zero vector alone ($Z$)  .
- **Subspaces of $\mathbb{R}^3$**:
  1.  All of $\mathbb{R}^3$  .
  2.  Any plane through the origin  .
  3.  Any line through the origin  .
  4.  The zero vector alone ($Z$)  .
-  **Invalid Subspaces**: Any line or plane that does **not** pass through the origin is not a subspace

## 4. Column Space
-  **Definition**: The column space $C(A)$ of a matrix $A$ consists of all linear combinations of its columns
-  **Example**: For $A = \begin{bmatrix} 1 & 3 \\ 2 & 3 \\ 4 & 1 \end{bmatrix}$, the column space is the plane through the origin in $\mathbb{R}^3$ containing the vectors $\begin{bmatrix} 1 \\ 2 \\ 4 \end{bmatrix}$ and $\begin{bmatrix} 3 \\ 3 \\ 1 \end{bmatrix}$
-  **Application**: Understanding the column space is a prerequisite for solving $Ax = b$