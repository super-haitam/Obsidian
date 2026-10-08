# Chapter 1
## Decomposition and Special graphs
### Peterson Graph
#### Definition
The **Peterson Graph** is the simple graph whose vertices are the 2-element subsets of a 5-element set and whose edges are the pairs of disjoint 2-element subsets.
![[Pasted image 20261005005154.png]]
#### Note:
Define \[5] = {1, 2, 3, 4, 5}. Each vertex is defined by two integers {a, b} such that {a, b} $\subset$ \[5].
One could see it as: A vertex {a, b} is adjacent to {x, y} *if and only if* {a, b} $\cap$ {x, y} = $\varnothing$.  
#### Proposition:
If two vertices are nonadjacent in the **Peterson Graph**, then they have exactly one common neighbor.
##### Proof
Let u and v be two different vertices in a **Peterson Graph**. If they are nonadjacent, then they share exactly one common integer, say u := {a, b} and v := {a, c} st. a $\neq$ c. Then, there are exactly two integers left in the \[5] / {a, b, c} representing the vertex, say, w. Thus, u and v have exactly one common neighbor w. $\blacksquare$ 

# Chapter 2
## Connection in graphs
### Recall
A **path** is a simple graph whose vertices are adjacent *if and only if* they appear consecutively in the list.
A **cycle** is a graph with equal number of vertices and edges whole vertices can be placed around a circle so that two vertices are adjacent *if and only if* they appear consecutively around the circle.
A **walk** is a list $v_{0}, e_{1}, v_{1}, ..., e_{k}, v_{k}$ of vertices and edges such that, for $1 \leq i \leq k$, the edge $e_{i}$ has endpoints $v_{i-1}$ and $v_{i}$.
A **trail** is a walk with no repeating edge.
A **u, v-walk** or **u, v-trail** has first vertex u and last vertex v.
The **length** of a walk, trail, path or cycle is its number of edges. A walk or trail is **closed** if its endpoints are the same.
### Lemma 
Every **u, v-walk** contains a **u, v-path**.
#### Proof
**Fully Self-Made**
Let $v_{0}, e_{1}, v_{1}, ..., e_{k}, v_{k}$ be a walk. 
If $k=0$; then a walk of length 0 is by definition a path.
Suppose the lemma is true up to length $k-1$, adding a new edge and vertex $e_{k}, v_{k}$. 
Let's consider the path $\{v_{0}, ..., v_{k-1}\}$ within the walk.
	If $v_{k} \notin \{v_{0}, ..., v_{k-1}\}$ with $m$ elements, thus we consider the walk to be a path.
	Otherwise, let $i$ be the index at which $v_{i} = v_{k}$ , then, by considering ${v_{0}, ..., v_{i}}$ , we conclude that is indeed a path with the same endpoints, affirming the hypothesis. $\blacksquare$

> [!note]- Detailed AI Proof
> The idea is that if a walk repeats a vertex, you can cut out the loop between the two visits, which makes the walk shorter, and then the induction applies.
> 
> **Proof.** We use strong induction on the length k of a u,v-walk: assume the lemma holds for all walks of length less than k.
>
> **Base case (k = 0).** The walk is just the single vertex u = v. It has no repeated vertices, so it is already a path.
>
>**Inductive step (k ≥ 1).** Let W = w₀, w₁, ..., w_k be a u,v-walk with w₀ = u and w_k = v.
>
>_Case 1: no vertex repeats in W._ Then W is itself a u,v-path, and we're done.
>
>_Case 2: some vertex repeats._ Then w_i = w_j for some i $\lt$ j.  Delete the part of the walk between the two visits:
>
>W' = w₀, ..., w_i, w_{j+1}, ..., w_k.
>
>This is still a walk. Each consecutive pair is an edge of W, and the one new pair, w_i followed by w_{j+1}, is an edge because w_i = w_j and w_j w_{j+1} was an edge of W. It still starts at u and ends at v, and its length is k − (j − i), which is less than k since j > i.
>
>By the inductive hypothesis, W' contains a u,v-path. Every vertex and edge of W' is also in W, so this path is contained in W too. ∎
>
>Strong induction is needed because the shortened walk can be much shorter than k − 1, so assuming the lemma only for length k − 1 wouldn't be enough.

### Components of a graph
#### Definition
A **maximal connected subgraph** of G is a subgraph that is connected and that is not contained in any other connected subgraph of G.
The **components** of a graph G are its maximal connected subgraphs. A component (or graph) is *trivial* if it has no edges; otherwise it is nontrivial.
An **isolated vertex** is a vertex of degree 0.
#### Proposition
Every graph with **n vertices** and **k edges** has at least **n - k components**.
##### Proof
Let's have a graph $G$ st. $|E(G)| = n$, being only formed of isolated vertices, it has n components. If we add a vertex, then it has exactly n - 1 components. At the third vertex, it has at least n - 2 components. By induction, since adding an edge leads to the number of components decreasing by at most one. Thus, at each step, when $|V(G)| = k$, G has at least $n - k$ components. 

>[!note]- Detailed AI Proof
>**Proof.** Induction on the number of edges k, with n fixed.
>
>**Base case (k = 0).** No edges means every vertex is its own component, so there are n components, which is n − 0. ✓
>
>**Inductive step (k ≥ 1).** Assume every graph with n vertices and k − 1 edges has at least n − (k − 1) components. Let G have n vertices and k edges. Pick any edge e = xy and let G' = G − e (delete the edge, keep all the vertices). Then G' has n vertices and k − 1 edges, so by the inductive hypothesis G' has at least n − k + 1 components.
>
>Now put e back to get G. Adding e only affects the components containing x and y. There are two possibilities:
>
> - If x and y are already in the same component of G', then e adds no new connections between components, so the number of components stays the same
> - If x and y are in different components of G', then e merges those two components into one, and the number of components drops by exactly 1.
>
>Every other component is untouched. So adding one edge lowers the number of components by at most 1. Therefore G has at least (n − k + 1) − 1 = n − k components. ∎
>
>If n − k is zero or negative, the statement is still true, just trivial, since any graph with at least one vertex has at least 1 component.
>

[TODO: Page 11/27]

