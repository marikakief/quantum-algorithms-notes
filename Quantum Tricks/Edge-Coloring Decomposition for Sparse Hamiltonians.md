
> **Source:** Berry, Ahokas, Cleve, Sanders, arXiv:quant-ph/0508139 (2005)
> **Tags:** #trick #sparse #decomposition #graph-coloring

## What it does

Decomposes a $d$-sparse Hamiltonian into a sum of [[1-Sparse Hamiltonian Simulation via 2×2 Blocks|1-sparse Hamiltonians]] using only local (black-box) access to matrix entries. No global knowledge of the sparsity pattern needed.

## The trick

1. View $H$ as a bipartite graph: rows on one side, columns on the other, edge for each nonzero off-diagonal entry.
2. A bipartite graph with max degree $d$ has a proper edge coloring with $d$ colors (König's theorem). Each color class is a matching → a [[1-Sparse Hamiltonian Simulation via 2×2 Blocks|1-sparse matrix]].
3. Computing the optimal $d$-coloring requires global graph knowledge. Instead, use a deterministic local coloring rule that assigns colors using only the neighbor function $f(x, i)$.
4. The local rule produces $6d^2$ colors (quadratic blowup over optimal) and costs $O(\log^* n)$ queries per color lookup.

Result: $H = \sum_{j=1}^{6d^2} H_j$, each $H_j$ one-sparse, each simulable with $O(\log^* n)$ calls to the black box.

```
  d-sparse H
       │
       ▼
  Local edge coloring (O(log* n) queries/color)
       │
       ▼
  6d² one-sparse terms:  H = H₁ + H₂ + ··· + H_{6d²}
       │
       ▼
  Feed into Suzuki integrator or any product formula
```

## Why $6d^2$ and not $d$

The optimal $d$-coloring from König's theorem requires solving a global matching problem. With only oracle access, the best known local procedure squares the color count. The $6$ is from the specific deterministic scheme used (related to iterated hash-based label disambiguation).

## When to reach for it

- Any [[Order-Condition Cancellation in Product Formulas|product-formula]] simulation of a sparse Hamiltonian given by a black box.
- When you need to reduce "sparse but not 1-sparse" to a sum of trivially simulable pieces.
- The $d^2$ overhead matters: if sparsity dependence is your bottleneck, quantum-walk methods ([[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes|Berry–Childs 2011]]) get this down to linear in $d$.

## Complexity

- Colors: $m = 6d^2$
- Queries per color lookup: $O(\log^* n)$
- Each 1-sparse term is then simulated via [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]

## Caveat

The quadratic blowup in $d$ feeds directly into the total simulation cost. For Hamiltonians where sparsity is moderate but not tiny, this overhead is significant. Later approaches ([[Quantum-Walk Isometry Encoding for Black-Box Hamiltonians|quantum walks]], [[Linear Combination of Unitaries (LCU)|LCU]]) avoid this decomposition entirely.

## Related notes

- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[1-Sparse Hamiltonian Simulation via 2×2 Blocks]]
- [[Black-Box Hamiltonian Simulation and Unitary Implementation (Berry-Childs 2011) — Paper Notes]]
- [[Magnitude-Banded Hamiltonian Decomposition]]
- [[Suzuki Order as a Tunable Knob for Time Scaling]]
