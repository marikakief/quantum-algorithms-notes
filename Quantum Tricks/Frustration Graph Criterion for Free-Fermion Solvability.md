# Frustration Graph Criterion for Free-Fermion Solvability

> **Source:** Chapman, Elman, Mann, arXiv:2305.15625
> **Tags:** #trick #free-fermion #frustration-graph #integrability #classical-simulation

## What it does
Provides a polynomial-time decidable structural test for whether a quantum spin Hamiltonian has an exact free-fermion solution, based purely on the anticommutation structure of its Pauli terms.

## The trick
Given $H = \sum_j b_j \sigma_j$, build the **frustration graph** $G$: vertices are Pauli terms, edges join anticommuting pairs. If $G$ is:

1. **Claw-free** — no induced $K_{1,3}$ (no vertex anticommuting with three mutually commuting terms)
2. **Contains a simplicial clique** — a clique $K_s$ where each vertex's neighbourhood in $G \setminus K_s$ also forms a clique

then $H$ is free-fermion solvable for **all** choices of coefficients $\{b_j\}$. The spectrum decomposes into $\alpha(G)$ single-particle energies (where $\alpha(G)$ is the independence number) within each symmetry sector.

Checking both conditions is polynomial-time (Chudnovsky-Seymour algorithm for claw-free structure).

## When to reach for it
- Testing whether a given spin model is integrable / classically solvable
- Designing new exactly solvable models (engineer a Hamiltonian whose frustration graph is SCF)
- Identifying which quantum simulation targets genuinely need a quantum computer (SCF models don't)

## Complexity
Deciding SCF: polynomial in the number of Hamiltonian terms. Computing the single-particle energies requires diagonalising cycle symmetries, which may be non-trivial in the thermodynamic limit.

## Caveat
Sufficient but not necessary — there exist free-fermion models outside the SCF class (non-generic solutions requiring fine-tuned coefficients). The criterion is **generic**: it works for all values of the coupling constants.

## Related notes
- [[A Unified Graph-Theoretic Framework for Free-Fermion Solvability (Chapman-Elman-Mann 2023) — Paper Notes]]
- [[Generalized Cycle Symmetries from Even Holes]]
- [[Krylov Subspace Fermion Mode Construction]]
- [[Grassmann Integral Framework for Free-Fermion Traces]]
