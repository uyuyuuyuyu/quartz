

# 📚 My Knowledge Base

## 💻 Computer System
* [[instructions]] — System Architecture and CPU




# Linear Algebra: Factorization into $A=LU$

This folder contains notes on the decomposition of a matrix $A$ into lower and upper triangular matrices, the fundamental subspaces that arise from these operations, and the computational logic behind solving linear systems.

## 📂 Core Concepts

* **[Factorization into $A=LU$](A=LU.md)**: The primary process of Gaussian elimination expressed as matrix multiplication.
* **[Permutation Matrices](permutation.md)**: How to handle row exchanges ($PA=LU$) when a zero appears in the pivot position.
* **[Solving $Ax=b$](Solving-Ax=b.md)**: Using the $LU$ decomposition to find solutions through forward and back substitution.

## 🧩 Vector Spaces & Subspaces

* **[Fundamental Subspaces](fundamental%20subspaces.md)**: Overview of the four essential subspaces: Column Space, Nullspace, Row Space, and Left Nullspace.
* **[Column Space and Nullspace](Column%20Space%20and%20Nullspace.md)**: Deep dive into the geometry of solutions and the span of matrix columns.
* **[Independence, Basis, and Dimension](Independence%20Basis%20and%20dimension.md)**: The criteria for spanning a space and the definition of dimension.
* **[Matrix Spaces and Rank](matrix%20spaces%20rank.md)**: Understanding matrices as elements of a vector space and the implications of rank.

## 🛠️ Procedural Details

* **[Pivot Variables and Special Solutions](Pivot%20Variables%20and%20Special%20Solutions.md)**: Step-by-step logic for finding the general solution to $Ax=b$.
* **[Matrix Notes](matrixNote.md)**: General observations and miscellaneous properties of matrix operations.

---

> [!INFO] Summary of Complexity
> For an $n \times n$ matrix, the elimination process to reach $U$ takes approximately $\frac{1}{3}n^3$ operations. Once $L$ and $U$ are known, solving for any vector $b$ is highly efficient, requiring only $n^2$ operations.

## 🔗 External Resources
* [Programming Languages](https://taura.github.io/programming-languages/) — Additional context on language implementations.


* [Numerical Analysis](https://ocw.u-tokyo.ac.jp/course_11479/)
