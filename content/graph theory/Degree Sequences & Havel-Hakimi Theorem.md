# Lecture Notes 2: Degree Sequences & Havel-Hakimi Theorem

## 1. Introduction to Degree Sequences
**Definition:** A degree sequence is a list (multiset) of the degrees of the vertices of a graph.

**Graphical Sequence:**
* A finite sequence of non-negative integers is called **graphical** if there exists a graph whose degrees match that list.
* *Note:* If a sequence is graphical, all of its permutations are also graphical.

### Fundamental Validity Checks
Before applying complex algorithms, use these theorems to check if a sequence is valid:

1.  **The Handshaking Lemma (Thm 2.1):**
    * For any graph $G=(V,E)$, the sum of degrees is twice the number of edges:
        $$\sum_{v \in V} d(v) = 2|E|$$
    * *Idea:* Every edge contributes 2 to the total sum of degrees.

2.  **Corollary 2.3:**
    * The degree sequence of every graph must have an **even number of odd elements**.

**Examples from Class:**
* `3, 3, 3, 1`: **Not Graphical**. (Contains 4 odd numbers, but notes mark "No, odd # of odd" or imply structural failure) .
* `7, 6, 9, 9...`: **Not Graphical**. The number of vertices must be greater than the maximum degree (e.g., a vertex with degree 7 requires at least 8 total vertices).

---

## 2. The Havel-Hakimi Theorem (Thm 2.10)
**Statement:** Let $n \ge 2$. A non-increasing sequence $S: d_0, d_1, \dots, d_{n-1}$ is graphical if and only if the reduced sequence $S_1$ is graphical.

### The Reduction Algorithm ($S \rightarrow S_1$)
To test if sequence $S$ is graphical:
1.  **Isolate:** Take the first element $d_0$ (degree of vertex $v$).
2.  **Reduce:** Subtract 1 from the next $d_0$ elements in the sequence.
3.  **Delete:** Remove $d_0$ from the list.
4.  **Repeat:** Reorder the new list to be non-increasing and repeat until the result is trivially graphical (e.g., all zeros) or invalid (e.g., negative numbers).

**Example:**
* **$S$**: `3, 3, 3, 2, 2, 2, 1`
* **Step 1**: Remove first `3`. Subtract 1 from next three (`3, 3, 2` $\rightarrow$ `2, 2, 1`).
    * Result: `2, 2, 1, 2, 2, 1`
    * Reorder ($S_1$): `2, 2, 2, 2, 1, 1`.
* **Step 2**: Remove first `2`. Subtract 1 from next two (`2, 2` $\rightarrow$ `1, 1`).
    * Result ($S_2$): `1, 1, 2, 1, 1` (Reorder to `2, 1, 1, 1, 1`).

---

## 3. Proof of Havel-Hakimi
**Goal:** Prove $S$ is graphical $\iff$ $S_1$ is graphical.

### Direction 1: $(\Leftarrow)$ If $S_1$ is graphical, then $S$ is graphical.
* **Assumption:** There exists a graph $G_1$ with degree sequence $S_1$.
* **Construction:** Create graph $G$ from $G_1$ by adding a new vertex $v_1$. Connect $v_1$ to the $d_1$ vertices that had their degrees reduced in the previous step.
* **Conclusion:** The new graph $G$ has degree sequence $S$.

### Direction 2: $(\Rightarrow)$ If $S$ is graphical, then $S_1$ is graphical.
* **Challenge:** We must prove there exists a realization of $S$ where the highest degree vertex $v_1$ connects *specifically* to the $d_1$ highest-degree neighbors.
* **Proof by Contradiction / Extremal Argument:**
    1.  Assume $S$ is graphical but no such "ideal" graph exists.
    2.  Select the graph $G$ (realizing $S$) where the sum of degrees of neighbors of $v_1$ is **maximized**.
    3.  **The Mismatch:** Since $G$ is not ideal, $v_1$ is connected to a "low degree" vertex $v_s$ but misses a "high degree" vertex $v_r$ (where $d_r > d_s$).
    4.  **The Swap:**
        * Find a vertex $v_t$ connected to $v_r$ but not $v_s$ (must exist because $d_r > d_s$).
        * **Remove edges:** $v_1v_s$ and $v_rv_t$.
        * **Add edges:** $v_1v_r$ and $v_sv_t$.
    5.  **Contradiction:** This new graph $G'$ has the same degree sequence $S$, but the neighbor sum for $v_1$ is higher (since $v_r$ replaces $v_s$). This contradicts the assumption that $G$ was maximized.