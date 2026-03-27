# Generalized Cycle Symmetries from Even Holes

> **Source:** Chapman, Elman, Mann, arXiv:2305.15625
> **Tags:** #trick #free-fermion #symmetry #frustration-graph #integrability

## What it does
Constructs a family of mutually commuting Hamiltonian symmetries from even holes in the frustration graph, decomposing the Hilbert space into sectors where free-fermion solutions exist independently.

## The trick
For an even hole $C$ of length $2k$ in the frustration graph, with coloring classes $C_a, C_b$ (each an independent set of size $k$), form the operator $h_C = h_{C_a} h_{C_b}$. Individual $h_C$ operators don't commute with $H$, but specific **signed sums over deformations** of even holes do.

A deformation replaces vertices along the hole while preserving the cycle structure. The **generalized cycle symmetry** $J^{\langle C_0 \rangle}_G$ is the sum over all deformations of a reference hole $C_0$. These satisfy:

- $[J^{\langle C_0 \rangle}_G, H] = 0$ for all even holes $C_0$
- $[J^{\langle C \rangle}_G, J^{\langle C' \rangle}_G] = 0$ for all pairs
- $[J^{\langle C \rangle}_G, Q^{(k)}_G] = 0$ where $Q^{(k)}_G$ are independent set charges

In Jordan-Wigner-type models (line graphs), cycle symmetries are Pauli operators ($J^2 \propto I$). In the general SCF case, they may satisfy higher-order polynomial relations (e.g., $J_1^2 = 6I - 2J_2$).

## When to reach for it
- Block-diagonalising free-fermion Hamiltonians into symmetry sectors
- Identifying hidden conservation laws in spin models from graph structure alone
- Generalising the conserved quantities of the Kitaev honeycomb model to broader classes

## Complexity
Identifying even holes and computing cycle symmetries is polynomial in graph size. Diagonalising the symmetries in the thermodynamic limit may require additional work.

## Caveat
When there are no even holes (e.g., (even-hole, claw)-free graphs), there's only one symmetry sector and the construction reduces to the method of Elman-Chapman-Flammia (2021). The non-trivial case is precisely when even holes are present — which is the new content.

## Related notes
- [[A Unified Graph-Theoretic Framework for Free-Fermion Solvability (Chapman-Elman-Mann 2023) — Paper Notes]]
- [[Frustration Graph Criterion for Free-Fermion Solvability]]
- [[Krylov Subspace Fermion Mode Construction]]
