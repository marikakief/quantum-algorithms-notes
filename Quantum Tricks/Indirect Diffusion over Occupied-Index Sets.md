# Indirect Diffusion over Occupied-Index Sets

> **Tags:** #trick #subspace-methods #fermions #block-encoding
> **Source:** arXiv:2510.08644

## What it does
Performs diffusion/superposition over occupied orbitals only, instead of all indices, for particle-number-conserving subspace algorithms.

## The trick
For an $\eta$-electron state, the physically relevant orbital transitions involve only the $\eta$ occupied orbitals. Instead of diffusing over all $n$ orbital indices (producing a $n$-dimensional uniform superposition), use an occupation oracle to map rank index $i \in [\eta]$ → occupied orbital index $j_i$, then diffuse over $i$. The diffusion lives in an $\eta$-dimensional space, not an $n$-dimensional one. Uncompute the occupation oracle after use. This is "indirect" because you never directly address orbital $j_i$ — you address rank $i$ and let the oracle translate.

## When to reach for it
- $\eta$-particle simulations where full-index diffusion is wasteful

## Complexity
Can materially reduce effective normalization $\alpha$ and downstream [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] cost.

## Caveat
Adds intricate oracle and uncomputation logic.

## Related Paper Notes
- [[Sublinear-T Block-Encodings for Second-Quantized Hamiltonians (arXiv 2510.08644) — Paper Notes]]
