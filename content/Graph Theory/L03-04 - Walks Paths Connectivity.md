# Graph Theory Lecture Notes

## 1. Walks, Paths, and Subgraphs

### **Definitions**
* **Subgraph:** If $V' \subseteq V$ and $E' \subseteq E$, then $G' = (V', E')$ is a subgraph of $G$ ($G' \subseteq G$).
* **Walk:** A sequence of vertices where every consecutive pair is an edge in $G$. If the vertices of a walk $W$ are vertices in $G$ and every consecutive pair in $W$ is an edge in $G$, we say $G$ contains $W$.
* **Path:** If a graph $G$ has a subgraph isomorphic to a path with endpoints $a$ and $b$, we say $G$ has an $a-b$ path.

### **Theorems on Walks and Paths**
* **Theorem 1.6:** If a graph $G$ contains a $u-v$ walk of length $l$, then it contains a $u-v$ path of length $\le l$.
    * **Idea of Proof:** Cut out middle bits between repeated vertices. If a walk is not a path, it has the form $W = ... v_i ... v_j ...$ where $v_i = v_j$. Removing the section between these indices yields a strictly shorter walk.
* **Corollary (Transitivity):** If $G$ has an $a-b$ path and a $b-c$ path, then $G$ has an $a-c$ path.
    * *Note:* This is done by concatenating paths.

---

## 2. Connectivity

### **Definitions**
* **Connected vs. Disconnected:** If for every $u, v \in V(G)$, there exists a $u-v$ path in $G$, $G$ is connected. If not, $G$ is disconnected.
* **Component:** A maximal connected subgraph. A connected graph has exactly one component.

### **Equivalence Relations (Theorem 1.7)**
* **Theorem 1.7:** Let $R$ be the relation defined on $V(G)$ as follows: $uRv$ iff $G$ contains a $u-v$ path. $R$ is an equivalence relation.
    * **Reflexivity:** $u-u$ is a path of length 0, so $uRu$.
    * **Symmetry:** If $uRv$, there is a $u-v$ path. Reading it backwards gives a $v-u$ path, so $vRu$.
    * **Transitivity:** If $uRv$ and $vRw$, then $uRw$ (Corollary to Thm 1.6).

---

## 3. Longest Paths and Vertex Removal

### **Properties of Longest Paths**
* **Proposition:**
$N(u)$ is the neighbouhood of u, Let $P$ be a longest path in a graph $G$, and let $u$ be an endpoint of $P$. Then $N(u) \subseteq V(P)$.
    * **Proof:** Suppose $N(u) \not\subseteq V(P)$. Then there exists $v \in N(u) \setminus V(P)$. This implies $Pv$ is a path of strictly greater length than $P$, which contradicts that $P$ is a longest path.

### **Connectivity Theorems Involving Deletion**
* **Theorem 1.8:** Let $G$ be a graph of order 3 or more. If $G$ contains two distinct vertices $u$ and $v$ such that $G-u$ and $G-v$ are connected, then $G$ itself is connected.  
   
* **Proof Strategy:**
    * **Case 1:** For vertices $x, y \notin \{u, v\}$, since $G-u$ is connected, there is an $x-y$ path in $G-u$, which implies they are connected in $G$ 
    * **Case 2:** To show $u$ and $v$ are connected, use a third vertex $w$ (exists since order $\ge 3$). Since $G-v$ is connected, there is a $u-w$ path. Since $G-u$ is connected, there is a $w-v$ path. Combining them produces a $u-v$ walk, ensuring they are connected.

* **Theorem 1.9 (Converse of 1.8):** Let $G$ be a graph of order $\ge 3$. If $G$ is connected, then there exist distinct $u, v \in V(G)$ such that $G-u$ and $G-v$ are connected.

    * **Proof Strategy:**
        1.  Let $u, v$ be endpoints of a longest path $P$ in $G$.
        2.  Since $|G| \ge 3$ and connected, $u \neq v$.
        3.  To show $G-u$ is connected: Let $x, y \in V(G-u)$. Since $G$ is connected, there is an $x-y$ path in $G$.
        4.  If the path does not touch $u$, it exists in $G-u$. If it does, using the property that neighbors of the endpoint $u$ are on $P$, we can reroute the connection through $P$.



* **Proposition (Leaves):** Suppose $G$ is a connected graph with a vertex $v$ of degree 1 (or generally $d(v) < 2$, implying a leaf/endpoint). Then $G-v$ is connected.
    * **Proof:** For any $x, y \in V(G)-v$, there is an $x-y$ path in $G$. Since $d(v) < 2$, $v$ cannot be an interior vertex of this path. Thus, the path exists in $G-v$.

---

## 4. Distance and Diameter

### **Definitions and Properties**
* **Distance ($d(u,v)$):** The length of a shortest $u-v$ path in $G$.
    * If $G$ is disconnected, distance is undefined for vertices in different components.
    * **Bounds:** In a connected graph of order $n$, $0 \le d(u, v) \le n-1$.
* **Geodesic:** A $u-v$ path of length $d(u, v)$ (the shortest possible length) is called a **geodesic**. * **Diameter ($\text{diam}(G)$):** The greatest distance between any two vertices in a connected graph.
    * **Property:** If $d(u, v) = \text{diam}(G)$ and $w \neq u, v$, then no $u-w$ geodesic can contain $v$. Otherwise, $d(u, w) > d(u, v) = \text{diam}(G)$, which is impossible.

### **Visualizing Distance**
* **Level Structure:** Vertices can be visualized based on their distances from a specific vertex $t$.
    * $t$ is at the top (distance 0).
    * Neighbors of $t$ are one level down (distance 1).
    * Neighbors of those vertices are at the next level (distance 2), and so on. 
### **Triangle Inequality**
* **Proposition:** The triangle inequality holds for distances in graphs: $d(a,b) + d(b,c) \ge d(a,c)$.
    * **Proof:** Concatenating an $a-b$ path (length $d(a,b)$) and a $b-c$ path (length $d(b,c)$) gives an $a-c$ walk. By Theorem 1.6, there exists an $a-c$ path of length $\le$ the walk length.

---

## 5. Degree Conditions and Subgraphs

### **Cycles**
* **Theorem:** Every graph of minimum degree $\delta(G) \ge 2$ has a cycle.
    * **Proof:** Let $P$ be a longest path and $u$ be an endpoint. By the proposition above, all neighbors of $u$ are on $P$. Since $d(u) \ge 2$, $u$ has a neighbor $v$ on $P$ other than the vertex immediately next to it. The edge $uv$ plus the path segment between them forms a cycle.

### **Dense Graphs (Diameter $\le 2$)**
* **Theorem 2.4:** Let $G$ be a graph of order $n$. If $d(u) + d(v) \ge n-1$ for every distinct, non-adjacent pair $u, v$, then diam$(G) \le 2$ (and $G$ is connected).
    * **Proof:** Suppose diam$(G) > 2$. Then there exist $x, y$ such that $xy \notin E(G)$ and they share no neighbors ($N(x) \cap N(y) = \emptyset$). This implies $|N(x)| + |N(y)| \le n-2$, contradicting the assumption.
* **Corollary 2.5:** If $\delta(G) \ge \frac{n-1}{2}$, then diam$(G) \le 2$.
* **Sharpness:** The bound is best possible. For example, two disjoint cliques ($K_{n/2} \cup K_{n/2}$) have degree sum $n-2$ but the graph is disconnected.