# QFT Diagonalization for Circulant Laplacians

> **Tags:** #trick #qft #circulant #pde #hamiltonian-simulation
> **Source:** Andrew M. Childs, Jin-Peng Liu, and Aaron Ostrander, *High-precision quantum algorithms for partial differential equations*, arXiv:2002.07868, Quantum **5**, 574 (2021)

## What it does
Simulates Laplacian-derived Hamiltonians by exact diagonalization in Fourier basis.

## The trick
Use QFT to diagonalize circulant operator, apply diagonal phase rotations, transform back. The same structure underlies [[QSFT Factorization into Diagonal + QFT + Diagonal]] for non-circulant shifted operators.

## When to reach for it
- Periodic finite-difference Laplacians / circulant blocks

## Complexity
Polylog-size transforms + structured phase cost; far cheaper than generic sparse simulation.

## Caveat
Needs explicit eigenphase structure; generic diagonal unitaries can be expensive.

## Related Paper Notes
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]]
