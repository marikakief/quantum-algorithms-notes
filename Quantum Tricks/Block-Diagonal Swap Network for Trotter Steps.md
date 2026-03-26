# Block-Diagonal Swap Network for Trotter Steps

> **Source:** McClean, Faulstich, Zhu, O'Gorman, Qiu, White, Babbush, Lin, arXiv:1909.00028
> **Tags:** #trick #Trotter #fermionic-swap #block-diagonal #circuit-compilation #linear-connectivity

## What it does

Implements a Trotter step of a block-diagonal electronic structure Hamiltonian (as produced by [[DG Blocking for Block-Diagonal Hamiltonians|DG blocking]]) in circuit depth $O(N_b n_\kappa^3)$, interpolating between the $O(N)$-depth [[Fermionic Swap Network|linear swap network]] for diagonal Hamiltonians and the $O(N^3)$-depth generalized swap network for fully general Hamiltonians.

## The trick

The two-electron terms in a block-diagonal Hamiltonian have a constraint: any four orbitals appearing together must draw two from block $\kappa$ and two from block $\kappa'$ (or all four from the same block). Additionally, the orbital spins must have even parity (all same, or two-and-two).

The swap network exploits both constraints through four stages:

1. **Intra-block, same-spin acquaintances:** A 4-complete swap network $K^4_{n_\kappa/2}$ within each half-block (spin-up and spin-down separately). This brings every set of 4 same-spin orbitals within a block adjacent for their interaction gate. Depth: $O(n_\kappa^3)$.

2. **Intra-block, mixed-spin acquaintances:** A double bipartite swap network on each full block, acquainting all sets of 4 orbitals with 2 spin-up and 2 spin-down within the same block. Depth: $O(n_\kappa^3)$.

3. **Intra-block permutation:** Reorder orbitals within each block in preparation for inter-block operations. Depth: $O(n_\kappa)$.

4. **Inter-block acquaintances:** $N_b$ alternating layers of balanced double bipartite swap networks between adjacent block pairs. Each layer acquaints the 4-orbital sets with 2 orbitals from each block and even spin parity. Each balanced double bipartite network also swaps the two blocks, so all $\binom{N_b}{2}$ block pairs are covered. Depth per layer: $O(n_\kappa^3)$. Total: $O(N_b n_\kappa^3)$.

The inter-block stage dominates: overall Trotter step depth is $O(N_b n_\kappa^3) = O(N_d n_\kappa^2)$.

## When to reach for it

- You're doing Trotter-based simulation of a chemistry Hamiltonian in a DG or other block-diagonal basis, and want near-term-compatible circuits with linear qubit connectivity.
- You want to interpolate between the extremes: $O(N)$ depth for diagonal basis (like [[Plane-Wave Dual Basis]]) and $O(N^3)$ depth for arbitrary basis. Block-diagonal structure gives you $O(N_b n_\kappa^3)$ with $n_\kappa$ bounded.
- The alternative — applying the fully general $O(N_d^3)$-depth swap network while ignoring block structure — wastes circuit depth on zero-valued interactions.

## Complexity

- Trotter step depth: $O(N_b n_\kappa^3)$.
- Two-qubit gates per step: $O(N_b^2 n_\kappa^4)$ (matching the number of non-zero two-electron integrals).
- Connectivity: linear nearest-neighbour (same as the original [[Fermionic Swap Network]]).
- When $n_\kappa$ is constant: depth $O(N_b) = O(N_d)$, gates $O(N_d^2)$.

## Caveat

The $O(n_\kappa^3)$ depth per inter-block interaction means the constant factor grows rapidly with block size. At $n_\kappa = 20$, each inter-block step involves $\sim 8000$ depth layers — practical only if the number of blocks $N_b$ is modest. For large block sizes, the low-rank factorization alternative (Appendix A of the source paper) may be more efficient, since each block interaction can be decomposed into $O(n_\kappa)$-rank Cholesky factors with bounded depth per factor.

## Related notes
- [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]]
- [[DG Blocking for Block-Diagonal Hamiltonians]]
- [[Fermionic Swap Network]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]]
