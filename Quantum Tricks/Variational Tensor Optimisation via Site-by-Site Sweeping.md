# Variational Tensor Optimisation via Site-by-Site Sweeping

> **Source:** Verstraete and Cirac, arXiv:cond-mat/0407066
> **Tags:** #trick #tensor-networks #PEPS #variational #optimisation

## What it does
Optimises a tensor network state (MPS, PEPS, or compressed MPS) by updating one tensor at a time, reducing the global optimisation to a sequence of local eigenvalue or linear-algebra problems.

## The trick
For a PEPS with tensors $\{A^s_{h,v}\}$, the energy $E = \langle\Psi|H|\Psi\rangle / \langle\Psi|\Psi\rangle$ is a ratio of two expressions, each quadratic in the components of any single tensor $A^s_{\bar{h},\bar{v}}$ (with all other tensors fixed). Writing the free tensor as a vector $x$:

$$E = \frac{x^\dagger \hat{H}_{\text{eff}} x}{x^\dagger \hat{N} x}$$

This is a generalised eigenvalue problem: find the $x$ that minimises the Rayleigh quotient. The matrices $\hat{H}_{\text{eff}}$ and $\hat{N}$ are constructed by contracting the rest of the network using [[Row-by-Row MPS Contraction for 2D Tensor Networks|the boundary MPS method]].

**Sweep procedure:** Start from site $(1,1)$, optimise its tensor, move to $(2,1)$, etc. After covering all sites, sweep back. Repeat until the energy converges. This is the PEPS generalisation of the DMRG sweep in 1D.

The same idea works for MPS compression: minimise $\||\psi_A\rangle - |\psi_B\rangle\|^2$ over the compressed MPS $|\psi_B\rangle$, one tensor at a time. Each step is a linear system (the cost is quadratic in the free tensor).

## When to reach for it
- Ground-state search with any tensor network ansatz (MPS, PEPS, tree tensor networks)
- MPS compression after MPO application (the inner loop of 2D PEPS contraction)
- Any variational optimisation where the cost function is multilinear in the tensor components

## Complexity
Each local optimisation step requires constructing $\hat{H}_{\text{eff}}$ and $\hat{N}$, then solving a generalised eigenvalue problem of dimension $dD^z$ (where $z$ is the coordination number: 2 for MPS, 4 for 2D PEPS). The network contraction dominates the cost. Total cost per sweep scales polynomially in $N$, $d$, and $D$.

## Caveat
The sweep converges to a local minimum, not necessarily the global one. Multiple random initialisations or physical heuristics (e.g., starting from imaginary-time-evolved states) help but don't guarantee global optimality. Also, each step requires recomputing the environment tensors, which for PEPS involves the approximate boundary MPS contraction — so errors from the contraction approximation feed into the optimisation.

## Related notes
- [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes]]
- [[Projected Entangled Pairs as Variational Ansatz]]
- [[Row-by-Row MPS Contraction for 2D Tensor Networks]]
- [[MPS Canonical Form for Efficient State Tracking]]
