
> **Tags:** #trick #boundary-conditions #pde #symmetry
> **Source:** Andrew M. Childs, Jin-Peng Liu, and Aaron Ostrander, *High-precision quantum algorithms for partial differential equations*, arXiv:2002.07868, Quantum **5**, 574 (2021)

## What it does
Handles non-periodic boundary conditions while preserving transform-friendly simulation structure.

## The trick
Embed problem into larger periodic/symmetric domain (image construction), then restrict to symmetry sector corresponding to desired BCs. Used in [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes|structured PDE simulation]] to retain QFT-based diagonalization.

## When to reach for it
- Dirichlet/Neumann conditions with FFT/QFT-oriented solvers

## Complexity
Retains fast diagonalization route at moderate embedding overhead.

## Caveat
Requires careful symmetry projection and normalization control.

## Related Paper Notes
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]]
