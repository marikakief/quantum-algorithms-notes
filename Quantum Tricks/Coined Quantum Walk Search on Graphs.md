# Coined Quantum Walk Search on Graphs

> **Source:** Shenvi, Kempe & Whaley (2003) for hypercube; Ambainis (2003) for element distinctness; Ambainis, Kempe & Rivosh (2004) for grids. Survey: Ambainis, arXiv:quant-ph/0403120
> **Tags:** #trick #quantum-walk #search #Grover #coin

## What it does

Finds a marked vertex on a graph $G$ in $O(\sqrt{N/M})$ steps (where $N$ = vertices, $M$ = marked vertices), using a discrete quantum walk with a coin register. Generalises Grover search to graphs with spatial structure.

## The framework

State space: $|v, e\rangle$ for vertex $v$ and incident edge $e$.

One step:
1. **Coin flip:** At unmarked vertices, apply Grover diffusion $D_m$ on the edge register. At marked vertices, apply $-I$ (phase flip).
2. **Shift:** $S|v, e\rangle = |v', e\rangle$ (move along edge $e$).

Start in uniform superposition $\frac{1}{\sqrt{N \cdot \deg}}\sum_{v,e} |v, e\rangle$. Run $O(\sqrt{N/M})$ steps. Measure.

**Grover diffusion coin:**

$$
(D_m)_{ij} = \begin{cases} -1 + 2/m & i = j \\ 2/m & i \neq j \end{cases}
$$

where $m = \deg(v)$. This is the unique real unitary that treats all edges symmetrically (up to sign).

## Key results

| Graph | Steps | Coin | Reference |
|---|---|---|---|
| Hypercube ($2^n$ vertices) | $O(\sqrt{N})$ | Grover diffusion | Shenvi-Kempe-Whaley 2003 |
| $d$-dim lattice, $d \geq 3$ | $O(\sqrt{N})$ | Grover diffusion | AKR 2004 |
| 2D lattice | $O(\sqrt{N\log N})$ | Grover diffusion | AKR 2004 |
| Johnson graph $J(N,r)$ | $O(N^{2/3})$ for element distinctness | Grover diffusion | Ambainis 2003 |

## When to reach for it

- Searching a spatially structured database (lattice, graph, network)
- Finding collisions, cliques, or subsets satisfying a property (via [[Walk on the Johnson Graph for Subset Search|Johnson graph walk]])
- Any problem where the search space has a natural graph structure and you want better than $O(\sqrt{N} \cdot \text{diameter})$

## Complexity

$O(\sqrt{N/M})$ walk steps. Each step costs: one coin operation + one shift + one oracle query (to check if marked). For element distinctness on $J(N,r)$: each step costs 1 query, total $O(N^{2/3})$.

## Caveat

The coin choice **matters**. On the 2D grid, Grover diffusion gives $O(\sqrt{N\log N})$ but other natural coins give no speedup. The analysis is usually by reduction to a walk on the line (via symmetry layers), and the coin determines whether the effective 1D walk has the right interference pattern.

Discrete coined walks can outperform continuous walks in low dimensions (discrete gives $\sqrt{N}$ on 3D grids; continuous fails below $d = 5$).

## Related notes

- [[Quantum Walks and Their Algorithmic Applications (Ambainis 2003) — Paper Notes]]
- [[Walk on the Johnson Graph for Subset Search]]
- [[Continuous-Time Quantum Walk Search]]
- [[Standard Amplitude Amplification]] — Grover as the non-spatial special case
- [[Quantum Speed-Up of Markov Chain Based Algorithms (Szegedy 2004) — Paper Notes]]
