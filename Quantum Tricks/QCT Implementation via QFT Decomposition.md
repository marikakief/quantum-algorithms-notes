# QCT Implementation via QFT Decomposition

> **Tags:** #trick #qct #qft #spectral-methods
> **Source:** Andrew M. Childs, Jin-Peng Liu, and Aaron Ostrander, *High-precision quantum algorithms for partial differential equations*, arXiv:2002.07868, Quantum **5**, 574 (2021)

## What it does
Realizes cosine-transform basis changes through QFT-compatible circuits.

## The trick
Rewrite QCT with QFT plus sparse diagonal/permutation gadgets. Compare [[QSFT Factorization into Diagonal + QFT + Diagonal]] for the analogous shifted-Fourier decomposition.

## When to reach for it
- Spectral PDE solvers / boundary-adapted bases

## Complexity
Polylogarithmic transform depth in grid size.

## Caveat
Constant factors can be nontrivial for multidimensional tensor products.

## Related Paper Notes
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]]
