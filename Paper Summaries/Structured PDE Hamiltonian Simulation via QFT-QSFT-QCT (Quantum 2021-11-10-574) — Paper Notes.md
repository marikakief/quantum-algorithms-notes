> **Source:** Andrew M. Childs, Jin-Peng Liu, and Aaron Ostrander, *High-precision quantum algorithms for partial differential equations*, arXiv:2002.07868, Quantum **5**, 574 (2021)  
> **Links:** [arXiv](https://arxiv.org/abs/2002.07868) · [Quantum](https://doi.org/10.22331/q-2021-11-10-574)  
> **Tags:** #hamiltonian-simulation #pde #qft #spectral-methods #qlsa

---

## One-Line Take

The PDE counterpart of the Berry-Childs-Ostrander-Wang precision improvement for ODEs: achieve poly(d, log(1/ε)) complexity for linear PDEs, eliminating the poly(1/ε) scaling that plagued earlier quantum PDE algorithms, by combining adaptive discretization with high-precision QLSA.

---

## What the paper does

Before this paper, quantum algorithms for linear PDEs had complexity polynomial in $1/\epsilon$ — the precision problem that [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes|HHL]] and its successors fixed for linear systems, and that [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry-Childs-Ostrander-Wang]] fixed for ODEs. Childs-Liu-Ostrander close the gap for PDEs.

The headline result: complexity poly(d, log(1/ε)) for linear PDEs in d spatial dimensions.

Note what this does *not* defeat: the exponential dependence on d is not eliminated (it's polynomial in d, which still becomes painful in high dimensions). But the precision scaling is fixed.

Two main algorithmic approaches are developed:

**1. Finite difference algorithm (for the Poisson equation)**  
Adaptive-order finite differences are used to discretize the PDE. The key is choosing the finite difference order carefully to balance the discretization error against the condition number of the resulting linear system. The resulting sparse linear system can then be fed into Childs-Kothari-Somma QLSA with polylogarithmic precision cost.

**2. Spectral algorithm (for general second-order elliptic equations)**  
Spectral methods expand the solution in a basis that diagonalizes (or nearly diagonalizes) the differential operator. For periodic boundary conditions, this is the Fourier basis, and the QFT diagonalizes the relevant circulant operators exactly. For Dirichlet/Neumann conditions, the discrete cosine or sine transform serves the same role and is implementable via QFT plus sparse permutation/phase corrections. The method-of-images construction extends this to non-periodic cases by embedding into a larger symmetric domain.

The spectral route is where the QFT-based circuit tricks are most visible: structured operators become diagonal in the transform basis, operator application reduces to phase rotations, and the overall [[Hamiltonian simulation]] cost is polylogarithmic in the grid size.

---

## Key complexity result

$$\text{Complexity} \sim \text{poly}(d,\, \log(1/\epsilon))$$

This matches the precision-efficiency of the ODE paper (1701.03684) and of the Childs-Kothari-Somma linear systems paper (1511.02306). The dimension dependence (polynomial in d) is better than nothing but leaves high-dimensional PDEs potentially expensive.

---

## Where this fits in the landscape

The paper completes a natural progression:
- Childs-Kothari-Somma (2015, 1511.02306): poly(log 1/ε) for linear *systems*  
- Berry-Childs-Ostrander-Wang (2017, 1701.03684): poly(log 1/ε) for linear *ODEs*  
- Childs-Liu-Ostrander (2021, 2002.07868): poly(d, log 1/ε) for linear *PDEs*  

In each case the core strategy is the same: encode the problem as a well-conditioned sparse linear system and apply high-precision QLSA. The interesting work is in the discretization engineering that makes this possible.

---

## Reusable tricks

- [[QFT Diagonalization for Circulant Laplacians]]
- [[QSFT Factorization into Diagonal + QFT + Diagonal]]
- [[QCT Implementation via QFT Decomposition]]
- [[Method-of-Images Boundary Embedding for Quantum PDE Solvers]]

---

## Caveats

- The quantum speedup is over precision and system size, not spatial dimension (poly-d is retained).
- Quantum-state output model: you get an amplitude-encoded approximation to the solution field, not pointwise values.
- Smoothness and ellipticity of the PDE, and boundary condition regularity, are load-bearing for the condition number bounds.
- The spectral approach requires structure (periodicity or symmetry); finite difference handles more general domains but at higher cost.

---

## References within this paper

- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes|Childs, Kothari & Somma (2017)]] — QLSA used as subroutine
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes|Berry, Childs, Ostrander & Wang (2017)]] — quantum ODE algorithm (related problem)
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — qubitization for structured block-encodings
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — [[QSVT Meta-Template|QSVT]]
- Childs, Liu & Ostrander (2021) — high-precision quantum PDE algorithms

---

## Cross-References

- [[Improved Quantum Linear Systems via Fourier and Chebyshev LCUs (Childs-Kothari-Somma 2015) — Paper Notes]]
- [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]
