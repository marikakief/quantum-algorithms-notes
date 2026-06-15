
> **Source:** Ortiz, Gubernatis, Knill & Laflamme, PRA 64, 022319 (2001); also [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes|Abrams & Lloyd (1999)]]; Zalka (1998)
> **Tags:** #trick #quantum-chemistry #simulation #QFT #basis-switching

## What it does

Simulates a Hamiltonian $H = T + V$ (kinetic + potential) by switching between momentum and position representations via QFT. In momentum space, $T$ is diagonal; in position space, $V$ is diagonal. Each is easy to exponentiate in its own basis.

## The trick

For a first-quantized system on a grid of $2^l$ points per particle, using the convention that the computational basis initially stores position:

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

Each Trotter step: $n$ QFTs and inverse QFTs (textbook exact cost $O(nl^2)$ gates, with approximate-QFT variants depending on precision) + $O(n^2)$ pair potential phases + $O(n)$ kinetic phases. The $O(n^2)$ count is the number of particle pairs, not the arithmetic gate cost of evaluating each pair potential; Coulomb phases can be dominated by reversible arithmetic.

## Caveat

Only works when $T$ and $V$ are diagonal in dual (Fourier-related) bases. Depending on sign conventions, QFT and inverse-QFT may be swapped relative to the displayed order. For non-local potentials or non-standard kinetic terms, you need a different decomposition. Also, the product-formula error from $[T,V]\neq0$ and higher nested commutators must be controlled.

## Related notes

- [[Quantum Simulation of Real-Space Dynamics (Childs-Leng-Li-Liu-Zhang 2022) — Paper Notes]] — provides tight complexity analysis for this trick applied to real-space Hamiltonian simulation; proves high-order product formulas using this switching are near-optimal
- [[Quantum Simulations of Fermionic Systems (Ortiz-Gubernatis-Knill-Laflamme 2001) — Paper Notes]]
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]]
- [[Structured PDE Hamiltonian Simulation via QFT-QSFT-QCT (Quantum 2021-11-10-574) — Paper Notes]]
- [[QFT Diagonalization for Circulant Laplacians]]
- [[Order-Condition Cancellation in Product Formulas]]
- [[Trotter Error via Nested Commutators for Real-Space Hamiltonians]]
- [[Split-Operator Real-Space Simulation on a Quantum Computer]]
