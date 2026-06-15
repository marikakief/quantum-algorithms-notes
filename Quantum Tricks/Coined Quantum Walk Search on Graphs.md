
> **Source:** Shenvi, Kempe & Whaley (2003) for hypercube; [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes|Ambainis (2003)]] for element distinctness; Ambainis, Kempe & Rivosh (2004) for grids. Survey: Ambainis, arXiv:quant-ph/0403120
> **Tags:** #trick #quantum-walk #search #Grover #coin

## What it does

Provides a framework for searching graphs with a discrete-time quantum walk and a coin register. On highly symmetric or otherwise search-friendly graphs, this can reproduce Grover-like behavior; on arbitrary graphs there is no universal $O(\sqrt{N/M})$ coined-walk theorem.

## The framework

State space: $|v, e\rangle$ for vertex $v$ and incident edge $e$.

One step:
1. **Coin flip:** At unmarked vertices, apply Grover diffusion $D_m$ on the edge register. At marked vertices, apply $-I$ (phase flip).
2. **Shift:** $S|v, e\rangle = |v', e\rangle$ (move along edge $e$).

For regular, symmetric examples one often starts in the uniform superposition $\frac{1}{\sqrt{N \cdot \deg}}\sum_{v,e} |v, e\rangle$, runs for the application-specific walk time, and then measures. The time and success probability come from the spectrum of the marked walk, not just from the number of marked vertices.

**Grover diffusion coin:**

$$
(D_m)_{ij} = \begin{cases} -1 + 2/m & i = j \\ 2/m & i \neq j \end{cases}
$$

where $m = \deg(v)$. Under the usual permutation-symmetry requirement on the incident edges, this is the standard nontrivial real diffusion coin, up to harmless phase/sign conventions.

## Key results

| Graph | Steps | Coin | Reference |
|---|---|---|---|
| Hypercube ($2^n$ vertices) | $O(\sqrt{N})$ | Grover diffusion | Shenvi-Kempe-Whaley 2003 |
| $d$-dim lattice, $d \geq 3$ | dimension-dependent; $d\ge 3$ has strong coined-walk speedups | Grover diffusion | AKR 2004 |
| 2D lattice | $O(\sqrt{N}\log N)$-type behavior depending on success-amplification convention | Grover diffusion | AKR 2004 |
| Johnson graph $J(N,r)$ | $O(N^{2/3})$ for element distinctness | Grover diffusion | Ambainis 2003 |

## When to reach for it

- Searching a spatially structured database (lattice, graph, network)
- Finding collisions, cliques, or subsets satisfying a property (via [[Walk on the Johnson Graph for Subset Search|Johnson graph walk]])
- Problems where the search space has a natural graph structure and the relevant walk spectrum/checking cost can actually be analyzed

## Complexity

There is no single graph-independent complexity formula. Each step costs a coin operation and a shift, while marked checking may be a separate oracle query or may be computed from stored data. For element distinctness on $J(N,r)$, setup costs $r$, updates cost constant queries, checking is query-free from stored data, and the optimized total is $O(N^{2/3})$.

## Caveat

The coin choice **matters**. On the 2D grid, Grover diffusion gives a near-quadratic speedup up to logarithmic factors, but other natural coins can give no speedup. The analysis is usually by reduction to an invariant subspace (often symmetry layers), and the coin determines whether the effective walk has the right interference pattern.

Discrete coined walks can outperform continuous walks in low dimensions (discrete gives $\sqrt{N}$ on 3D grids; continuous fails below $d = 5$).

Do not use this card as a theorem that coined walks find marked vertices on every graph in Grover time. For general Markov-chain search, use Szegedy/MNRS-style parameters: setup, update, checking, spectral gap, and marked stationary measure.

## Related notes

- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]]
- [[Walk on the Johnson Graph for Subset Search]]
- [[Continuous-Time Quantum Walk Search]]
- [[Standard Amplitude Amplification]] — Grover as the non-spatial special case
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
