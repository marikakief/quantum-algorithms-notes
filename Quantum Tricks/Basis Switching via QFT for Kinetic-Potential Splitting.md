
> **Source:** Ortiz, Gubernatis, Knill & Laflamme, PRA 64, 022319 (2001); also [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams & Lloyd (1999)]]; Zalka (1998)
> **Tags:** #trick #quantum-chemistry #simulation #QFT #basis-switching

## What it does

Simulates a Hamiltonian $H = T + V$ (kinetic + potential) by switching between momentum and position representations via QFT. In momentum space, $T$ is diagonal; in position space, $V$ is diagonal. Each is easy to exponentiate in its own basis.

## The trick

For a first-quantized system on a grid of $2^l$ points per particle:

1. Start in position space
2. Apply $e^{-iV\delta t}$ — diagonal, just phase gates on position register
3. QFT to momentum space
4. Apply $e^{-iT\delta t}$ — diagonal in momentum, just phase gates
5. Inverse QFT back to position space
6. Repeat via [[Order-Condition Cancellation in Product Formulas|Trotter splitting]]

For $n$ particles with pairwise interactions $V_{ij}$: do the QFT per-particle ($n$ separate QFTs, each on $l$ qubits), apply kinetic phases, inverse QFT back, then apply potential phases.

## When to reach for it

- First-quantized quantum chemistry (the [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams-Lloyd]] setting)
- Any lattice simulation where kinetic and potential terms are diagonal in dual bases
- PDE simulations with Laplacian + potential structure ([[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes|Childs-Liu]])

## Complexity

Each Trotter step: $n$ QFTs ($O(nl^2)$ gates) + $O(n^2)$ potential phases + $O(n)$ kinetic phases. The QFT cost is logarithmic in grid size — much cheaper than matrix exponentiation.

## Caveat

Only works when $T$ and $V$ are diagonal in dual (Fourier-related) bases. For non-local potentials or non-standard kinetic terms, you need a different decomposition. Also, the Trotter error from $[T, V] \neq 0$ must be controlled.

## Related notes

- [[Quantum Simulations of Fermionic Systems (Ortiz-Gubernatis-Knill-Laflamme 2001) — Paper Notes]]
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]]
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]]
- [[QFT Diagonalization for Circulant Laplacians]]
- [[Order-Condition Cancellation in Product Formulas]]
