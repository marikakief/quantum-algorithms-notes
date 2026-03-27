> **Tags:** #trick #qsft #basis-change #pde
> **Source:** Andrew M. Childs, Jin-Peng Liu, and Aaron Ostrander, *High-precision quantum algorithms for partial differential equations*, arXiv:2002.07868, Quantum **5**, 574 (2021)

## What it does
Implements shifted Fourier transforms efficiently via simple factors around a standard QFT.

## The trick
Decompose QSFT as diagonal phase correction, QFT, and diagonal/permutation correction. Compare [[QFT Diagonalization for Circulant Laplacians]] (unshifted circulant case) and [[QCT Implementation via QFT Decomposition]] (cosine transform case).

## When to reach for it
- PDE discretizations where operator is simple in shifted-frequency basis

## Complexity
Keeps basis-change overhead polylogarithmic.

## Caveat
Bookkeeping for shifts/phases must stay coherent across tensor dimensions.

## Related Paper Notes
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]]
