> **Source:** Frédéric Magniez, Miklos Santha, and Mario Szegedy, *Quantum algorithms for the triangle problem*, SIAM Journal on Computing **37**(2):413–424, 2007; arXiv:quant-ph/0310134
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0310134) · [SIAM](https://doi.org/10.1137/05064975X)
> **Tags:** #triangle-finding #quantum-walk #Grover #query-complexity #graph-property #element-distinctness

---

## The computational problem

**Triangle Finding:** Given the adjacency matrix of a graph $G$ on $n$ nodes (as an oracle), find a triangle (copy of $K_3$) or reject if $G$ is triangle-free.

Query complexity is measured by the number of oracle queries to entries of the adjacency matrix. The trivial classical bound is $O(n^2)$ (read the whole matrix). Classically, the randomised query complexity is $\Theta(n^2)$. Quantumly, the trivial Grover-based approach gives $O(n^{3/2})$ — pick a pair $(u,v)$ in superposition, check if an edge $(u,v)$ exists and if so search for a common neighbour.

Prior work: Buhrman, Dürr, Heiligman, Høyer, Magniez, Santha, and de Wolf (2005) gave $O(n + \sqrt{nm})$ which improves on $O(n^{3/2})$ for sparse graphs but breaks down for dense graphs ($m = \Theta(n^2)$).

---

## What the paper does

Gives two algorithms that break the $O(n^{3/2})$ barrier for triangle finding:

| Algorithm | Query complexity | Qubits | Technique |
|---|---|---|---|
| Combinatorial | $\tilde{O}(n^{10/7})$ | $O(\log n)$ quantum + $O(n^2)$ classical | Grover + combinatorial classification |
| Quantum walk | $\tilde{O}(n^{13/10})$ | $O(n)$ | [[Walk on the Johnson Graph for Subset Search\|Ambainis walk]] + recursion |

The second algorithm is the more interesting one. It generalises [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis's element distinctness algorithm]] by using a *recursive* quantum walk: the checking subroutine inside the walk is itself a quantum walk (Graph Collision), giving a nested structure.

Both algorithms improve on the $O(n^{3/2})$ Grover bound. Later work by Le Gall (2014) improved this to $O(n^{5/4})$, and the current best is $\tilde{O}(n^{5/4})$, still short of the $\Omega(n)$ lower bound.

---

## Algorithm 1: Combinatorial approach ($\tilde{O}(n^{10/7})$)

### Key combinatorial ideas

**Notation:** For vertices $a, b$, let $t(G, a, b) = |\{x : (a,x) \in G \text{ and } (b,x) \in G\}|$ be the number of length-2 paths from $a$ to $b$. Define $G\langle k \rangle = \{(a,b) \in [n]^2 : t(G, a, b) \leq k\}$.

**Lemma 2 (neighbourhood cleaning):** For any vertex $v$, using $\tilde{O}(n)$ queries we either find a triangle or verify that the neighbourhood $\nu_G(v)$ contains no edge of $G$ (i.e., $G \subseteq [n]^2 \setminus \nu_G(v)^2$). Query all edges from $v$ classically ($n-1$ queries), then Grover search for an edge within $\nu_G(v)$.

**Lemma 3 (random cleaning):** Picking $k = \lceil 4n^\varepsilon \log n \rceil$ random vertices and cleaning their neighbourhoods ensures that all surviving edges $(a,b)$ satisfy $t(G, a, b) \leq n^{1-\varepsilon}$ with high probability.

After cleaning, the algorithm classifies remaining edges into:
- **$T$**: edges with few length-2 paths — $T$ contains $O(n^{3-\varepsilon'})$ triangles, which can be searched by Grover in $\tilde{O}(\sqrt{n^{3-\varepsilon'}})$ queries
- **$E$**: edges incident to high/low degree vertices — $|E \cap G| = O(n^{2-\delta} + n^{2-\varepsilon+\delta+\varepsilon'})$, so triangles touching $E$ can be found by [[Amplitude Estimation via Phase Estimation on Q|amplitude amplification]]

**Optimising** with $\varepsilon = 3/7$, $\varepsilon' = \delta = 1/7$ gives $\tilde{O}(n^{10/7})$.

### What's clever about it

The quantum content is minimal — just Grover search. The improvement over $O(n^{3/2})$ comes entirely from the combinatorial observation that dense graphs (which are hard for Grover) must have many length-2 paths, which makes the cleaning step effective.

---

## Algorithm 2: Quantum walk approach ($\tilde{O}(n^{13/10})$)

### The Generic Algorithm framework

This paper presents [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis's quantum walk]] in a general "$k$-Collision" framework. Given:
- A set $S$ of size $n$
- An oracle function $f$ defining a collision relation $C \subseteq S^k$
- A database $D(A)$ for each subset $A \subseteq S$ of size $r$
- A checking procedure $\Phi(D(A))$ that detects collisions in $A$

The Generic Algorithm maintains the quantum state $|A\rangle|D(A)\rangle|x\rangle$ (set register, data register, coin register) and performs $\Theta((n/r)^{k/2})$ iterations, each consisting of:
1. Check: apply phase flip if $\Phi(D(A))$ accepts
2. Walk: do $\Theta(\sqrt{r})$ steps of [[Walk on the Johnson Graph for Subset Search|quantum walk on the Johnson graph]]

**Total cost:** $O(s(r) + (n/r)^{k/2} \cdot (c(r) + \sqrt{r} \cdot u(r)))$

where $s(r)$, $u(r)$, $c(r)$ are the setup, update, and checking costs.

### Graph Collision as a building block

The paper introduces **Graph Collision**: given a graph $G$ (known) and a boolean function $f$ on $[n]$ (oracle), find $(u, u')$ with $f(u) = f(u') = 1$ and $(u, u') \in G$. This is solved in $\tilde{O}(n^{2/3})$ by the Generic Algorithm with $s(r) = r$, $u(r) = 1$, $c(r) = 0$ (because checking is internal to the stored data), optimised at $r = n^{2/3}$.

### Recursive application to Triangle

Triangle is a 2-Collision problem where the collision relation is "$(u,v)$ is an edge of a triangle." The costs:

| Cost | Value | Reason |
|---|---|---|
| $s(r)$ | $O(r^2)$ | Load all edges within the $r$-vertex subset |
| $u(r)$ | $r$ | Adding/removing a vertex requires querying its edges to all $r$ existing vertices |
| $c(r)$ | $\tilde{O}(\sqrt{n} \cdot r^{2/3})$ | **Recursive:** for each candidate vertex $v$ (Grover search over $[n]$), run Graph Collision on $G|_A$ with $f(u) = [(u,v) \in G]$ |

The checking cost is where the recursion happens: the inner Graph Collision is itself a quantum walk. Optimising at $r = n^{3/5}$:

$$\tilde{O}\left(r^2 + \frac{n}{r}\left(\sqrt{n} \cdot r^{2/3} + \sqrt{r} \cdot r\right)\right) = \tilde{O}(n^{13/10})$$

### Extension to $k$-vertex subgraph finding

For finding copies of a fixed graph $H$ with $k > 3$ vertices, the same recursive approach gives $\tilde{O}(n^{2-2/k})$. The optimal $r = n^{1-1/k}$ makes the bound independent of the degree of the distinguished vertex in $H$.

---

## Key results

$$\text{Triangle:} \quad \tilde{O}(n^{10/7}) \text{ queries (combinatorial)}, \quad \tilde{O}(n^{13/10}) \text{ queries (quantum walk)}$$

$$\text{$k$-vertex subgraph:} \quad \tilde{O}(n^{2-2/k}) \text{ queries}$$

$$\text{Monotone graph property with $k$-vertex 1-certificates:} \quad \tilde{O}(n^{2-2/k}) \text{ queries}$$

The $\tilde{O}(n^{13/10})$ for Triangle was the best known at publication. Lower bound: $\Omega(n)$ (the adversary method is stuck at $\Omega(\sqrt{NK})$ and for Triangle $K = 3$, $N = n^2$, giving only $\Omega(n)$).

---

## Comparison with subsequent work

| Paper | Query complexity | Technique |
|---|---|---|
| This paper (2005/2007) | $\tilde{O}(n^{13/10})$ | Recursive quantum walk |
| Belovs (2012) | $O(n^{35/27})$ | Learning graphs |
| Le Gall (2014) | $\tilde{O}(n^{5/4})$ | Improved quantum walk with non-uniform data structures |
| Lower bound | $\Omega(n)$ | Adversary method (tight for this method) |

The gap between $\tilde{O}(n^{5/4})$ and $\Omega(n)$ remains open.

---

## Limits / caveats

- The $\tilde{O}$ hides polylogarithmic factors throughout.
- The quantum walk algorithm uses $O(n)$ qubits — significantly more than the $O(\log n)$ qubits of the combinatorial algorithm.
- The lower bound $\Omega(n)$ may not be tight. The adversary method (all variants) cannot prove anything better than $\Omega(n)$ for Triangle because of the small certificate size ($K = 3$). Proving tighter lower bounds requires different techniques.
- The paper only addresses query complexity, not gate complexity. The quantum walk steps require additional computation to maintain the data structure.

---

## Reusable ideas

1. [[Recursive Quantum Walk for Nested Collision Problems]] — Using a quantum walk as the *checking subroutine* inside another quantum walk. This recursive structure handles problems where detecting a collision requires solving a sub-problem that is itself a structured search. The outer walk explores subsets; the inner walk (Graph Collision) checks each subset.

2. [[Neighbourhood Cleaning for Dense Graph Properties]] — The combinatorial technique of randomly sampling vertices, reading their full neighbourhoods, and removing edges within those neighbourhoods. This reduces the problem to a sparser instance where the remaining edges have bounded length-2 path counts, enabling more efficient search.

---

## References within this paper

- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover (1996)]] — base search subroutine
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2004/2007)]] — the quantum walk on the Johnson graph that this paper generalises
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Brassard-Høyer-Mosca-Tapp (2002)]] — amplitude amplification used throughout
- Buhrman et al. (2005) — prior $O(n + \sqrt{nm})$ result for sparse Triangle
- Aaronson-Shi (2004) — $\Omega(n^{2/3})$ lower bound for collision via polynomial method
- Szemerédi (1976) — regularity lemma (mentioned but not used; simpler observation suffices)
- Childs-Eisenberg (2003) — independent $k$-clique finding result

---

## Cross-links

### Paper notes
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]] — the framework this paper builds on
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — the other quantum walk framework; Szegedy is a co-author
- [[Search via Quantum Walk (Magniez-Nayak-Roland-Santha 2007) — Paper Notes]] — unifies and refines the walk search framework; Magniez and Santha are co-authors
- [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes]] — base subroutine

### Trick cards
- [[Recursive Quantum Walk for Nested Collision Problems]]
- [[Neighbourhood Cleaning for Dense Graph Properties]]
- [[Walk on the Johnson Graph for Subset Search]]
