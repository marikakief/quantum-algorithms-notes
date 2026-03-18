# Column-Subspace Reduction for Symmetric Graphs

> **Source:** Childs, Cleve, Deotto, Farhi, Gutmann, Spielman, arXiv:quant-ph/0209131 (2003)
> **Tags:** #trick #quantum-walk #symmetry #graph-traversal

## What it does
Reduces a quantum walk on a large graph to an equivalent walk on a much smaller one by exploiting the fact that all vertices in each "column" (level set) are structurally equivalent.

## The trick

If a graph has columns $0, 1, \ldots, L$ such that every vertex in column $j$ connects to the same number of vertices in column $j+1$ (and $j-1$), then the uniform superposition over each column,

$$|\text{col } j\rangle = \frac{1}{\sqrt{N_j}} \sum_{a \in \text{column } j} |a\rangle$$

spans an **invariant subspace** of the adjacency Hamiltonian $H$. A quantum walk starting in $|\text{col } 0\rangle$ stays in this $(L+1)$-dimensional subspace forever.

The effective 1D Hamiltonian has matrix elements $\langle \text{col } j | H | \text{col}(j+1) \rangle = \gamma \sqrt{d_{j \to j+1}}$, where $d_{j \to j+1}$ is the number of edges from each vertex in column $j$ to column $j+1$. For balanced binary trees, this gives uniform coupling everywhere except possibly at the join.

This works even when the inter-column connections are random — as long as the degree pattern is uniform within each column. The random cycle in $G'_n$ preserves this balance.

## When to reach for it
- Quantum walks on graphs with a "layered" or "levelled" structure (trees, lattices with distinguished boundary)
- Analysing hitting times — reduce to 1D and use Bessel function / scattering theory
- Any graph where each level set is a single orbit of the automorphism group

## Complexity
Reduces the analysis from an $N$-dimensional system to an $(L+1)$-dimensional one. The walk on the reduced line can often be solved analytically (Bessel functions, transfer matrices, etc.).

## Caveat
Requires starting in a column state (e.g., a single vertex in column 0). If the initial state has non-trivial structure within a column, the reduction doesn't apply. Also, the "columns" must be a genuine invariant decomposition — every vertex in the column must have the same degree pattern to adjacent columns.

## Related notes
- [[Exponential Algorithmic Speedup by Quantum Walk (Childs-Cleve-Deotto-Farhi-Gutmann-Spielman 2003) — Paper Notes]]
- [[Continuous-Time Quantum Walk Search]]
- [[On the Relationship Between Continuous- and Discrete-Time Quantum Walk (Childs 2010) — Paper Notes]]
