> **Source:** Andris Ambainis, *Quantum walks and their algorithmic applications*, Int. J. Quantum Inf. **1**(4), 507–518 (2003); arXiv:quant-ph/0403120
> **Links:** [arXiv](https://arxiv.org/abs/quant-ph/0403120) · [IJQI](https://doi.org/10.1142/S0219749903000383)
> **Tags:** #quantum-walk #survey #element-distinctness #search #hitting-time

---

## What the paper does

A compact survey of quantum walks and their algorithmic applications as of 2003–2004. Covers both discrete and continuous-time quantum walks, then classifies known algorithms into two families:

1. **Exponentially faster hitting times** — reaching a target vertex on structured graphs
2. **Quantum walk search** — finding marked vertices on graphs (generalizing Grover)

This is the paper that introduced the framework for thinking about quantum walk algorithms as a unified toolkit.

---

## Quantum walks: the basics

### Discrete walk on the line

A naive quantum analogue of a random walk ($|n\rangle \to a|n-1\rangle + b|n\rangle + c|n+1\rangle$) doesn't work — the only unitary options are trivial (always go left, stay, or go right). Solution: add a **coin register**.

State space: $|n, c\rangle$ with $n \in \mathbb{Z}$, $c \in \{0, 1\}$. One step = coin flip $C$ then shift $S$:

$$
S|n, 0\rangle = |n-1, 0\rangle, \qquad S|n, 1\rangle = |n+1, 1\rangle
$$

With the Hadamard coin, the walk spreads **linearly** in $t$ steps (vs $\sqrt{t}$ classically). After $t$ steps, $\Omega(t)$ locations each have $\Omega(1/t)$ probability → expected distance from origin is $\Omega(t)$, quadratically faster than classical.

### Continuous walk

Defined via Hamiltonian $H = L$ (graph Laplacian). Evolution: $e^{-iHt}$. No coin needed.

### Walks on general graphs

**Discrete:** States $|v, e\rangle$ (vertex $v$, incident edge $e$). Coin flip $C_v$ on edge states at each vertex, then shift $S: |v, e\rangle \to |v', e\rangle$. Natural coin: Grover diffusion $D_m$ (the only non-trivial real unitary treating all edges equally).

**Continuous:** $H_{ij} = -1$ for adjacent $i,j$; $H_{ii} = \deg(i)$. Evolve $e^{iHt}$.

---

## Family 1: Exponentially faster hitting

### Glued trees (Childs-Farhi-Gutmann 2002/2003)

Two binary trees of depth $d$ with leaves glued together. $O(2^d)$ vertices. Classical random walk: $\Omega(2^d)$ steps to go from root $A$ to root $B$. **Continuous quantum walk: $O(d^2)$ steps** — exponential speedup.

**Technique:** Reduce to walk on the line by partitioning vertices into layers $S_{-d}, \ldots, S_d$ by distance from $A$. The walk in layer-space is a 1D quantum walk.

**Caveat:** Requires the graph structure to be unknown (unlabelled edges). If you know the graph, you can find $B$ classically by just following the tree structure. The speedup is over algorithms that can only explore locally.

### Open problem (as of 2003)

No natural computational problem was known where exponentially faster hitting gives a speedup. (This was partially addressed later by Childs 2009 for graph traversal problems.)

---

## Family 2: Quantum walk search

### Search on the hypercube (Shenvi-Kempe-Whaley 2003)

$N = 2^n$ items on an $n$-dimensional hypercube. Discrete walk with Grover diffusion coin, modified at marked vertices. Finds a marked item in $O(\sqrt{N})$ steps — matching Grover.

Analysis: reduce to walk on the line by Hamming weight layers (like glued trees).

### Search on grids

| Dimension $d$ | Continuous walk (Childs-Goldstone) | Discrete walk (Ambainis-Kempe-Rivosh) |
|---|---|---|
| $\geq 5$ | $O(\sqrt{N})$ | $O(\sqrt{N})$ |
| $4$ | $O(\sqrt{N\log N})$ | $O(\sqrt{N})$ |
| $3$ | No speedup | $O(\sqrt{N})$ |
| $2$ | No speedup | $O(\sqrt{N\log N})$ |

Surprising: **discrete walks beat continuous walks** in low dimensions. The coin provides extra structure that helps.

Also surprising: the choice of coin matters. Some natural coins give no speedup at all.

### Element distinctness (Ambainis 2003)

Given $x_1, \ldots, x_N$, are all values distinct? Equivalently, find $i \neq j$ with $x_i = x_j$.

**Key idea:** Walk on the Johnson graph $J(N, r)$ — vertices are subsets $S \subseteq [N]$ of size $r$. Each vertex $S$ stores $\{(i, x_i) : i \in S\}$. A marked vertex contains a collision.

One step of the walk: swap one element of $S$ for a random element not in $S$. Cost:
- Checking if marked: 0 queries (check the stored data)
- Moving to adjacent vertex: 1 query (look up $x_i$ for the new element)

**Result:** $O(N^{2/3})$ queries — tight (matches Shi's lower bound). Optimal $r = N^{2/3}$.

Compare with [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|BHT collision finding]]: $O(N^{1/3})$ with an $r$-to-1 promise vs $O(N^{2/3})$ without the promise.

**Generalizations:**
- **$k$-element distinctness** (find $k$ equal items): $O(N^{k/(k+1)})$ queries
- **Triangle finding** in a graph on $n$ vertices: $O(n^{1.3})$ queries
- **$k$-clique finding:** via reduction to $k$-element distinctness

---

## The quantum walk search framework

The common structure across all walk search algorithms:

1. Define a graph $G$ on the search space (items, subsets, configurations)
2. Choose a coin (Grover diffusion is the default)
3. Marked vertices satisfy the search criterion
4. At marked vertices, apply $-I$ to the coin register (a phase flip, like Grover's oracle)
5. Run for $O(\sqrt{N/M})$ steps where $M$ = number of marked vertices
6. Measure

This is a **quantum analogue of random-walk-based search**: classically, a random walk on $G$ with checking at each vertex finds a marked item in $O(T_{\text{hit}}/\epsilon)$ steps (where $\epsilon$ = fraction of marked vertices, $T_{\text{hit}}$ = hitting time). Quantumly, the walk achieves a quadratic improvement.

---

## Reusable ideas

1. **[[Coined Quantum Walk Search on Graphs]]:** Define a walk on the natural graph of the search space, use Grover diffusion coin with phase flip at marked vertices. The walk explores quadratically faster than classical, and the coin choice determines whether full $\sqrt{N}$ speedup is achieved. The framework for element distinctness, triangle finding, and spatial search.

2. **[[Walk on the Johnson Graph for Subset Search]]:** For problems where the answer is a small subset $S \subseteq [N]$ satisfying some property, walk on $J(N, r)$ (the graph of $r$-element subsets with Hamming distance 1). Each step queries one new element. With $r$ tuned to $N^{k/(k+1)}$, achieves $O(N^{k/(k+1)})$ for $k$-element problems.

---

## References within this paper

- Aharonov, Ambainis, Kempe & Vazirani (2001) — discrete quantum walks on graphs
- Farhi & Gutmann (1998) — continuous-time quantum walk definition
- Childs, Farhi & Gutmann (2002) — exponential hitting on glued trees
- Shenvi, Kempe & Whaley (2003) — walk search on hypercube
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes|Childs & Goldstone (2004)]] — continuous walk search on lattices
- Ambainis, Kempe & Rivosh (2004) — coined walk search; coins make walks faster
- Shi (2002) — $\Omega(N^{2/3})$ lower bound for element distinctness
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|Grover (1996)]] — the original quantum search; walk search generalises it
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes|Ambainis (2007)]] — the full element distinctness paper; $O(N^{2/3})$ via Johnson graph walk

---

## Cross-links

### Paper notes
- [[Spatial Search by Quantum Walk (Childs-Goldstone 2004) — Paper Notes]] — continuous walk search on lattices (reviewed in §5.2)
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]] — formalises the discrete walk framework; proves $\sqrt{\delta\epsilon}$ speedup rule
- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]] — collision finding with $r$-to-1 promise (vs element distinctness without promise)
- [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes]] — amplitude amplification that walk search generalises

### Trick cards
- [[Coined Quantum Walk Search on Graphs]]
- [[Walk on the Johnson Graph for Subset Search]]
- [[Continuous-Time Quantum Walk Search]]
- [[Standard Amplitude Amplification]] — the non-walk alternative
- [[Classical Preprocessing plus Grover Search]] — BHT's hybrid approach for collisions
