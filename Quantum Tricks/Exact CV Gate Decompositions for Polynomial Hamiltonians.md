# Exact CV Gate Decompositions for Polynomial Hamiltonians

> **Source:** Arrazola, Kalajdzievski, Weedbrook, Lloyd, arXiv:1809.02622
> **Tags:** #trick #continuous-variable #gate-decomposition #Hamiltonian-simulation #exact

## What it does
Decomposes multi-mode polynomial CV unitaries (trilinear and quadratic-linear products like $e^{i\delta\hat{X}_j\hat{X}_k\hat{X}_l}$, $e^{i\delta\hat{X}_j^2\hat{X}_k\hat{X}_l}$) into a finite number of gates from a universal CV gate set, with **zero approximation error**.

## The trick

The core identity exploits displacement conjugation — the fact that translating the position operator by another mode's position turns a single-mode polynomial into a multi-mode polynomial:

$$e^{i2\hat{P}_j\hat{X}_k}\, e^{i\delta\hat{X}_j^3}\, e^{-i2\hat{P}_j\hat{X}_k} = e^{i\delta(\hat{X}_j + \hat{X}_k)^3}$$

By carefully choosing a sequence of displacements and cubic phase gates across multiple modes, then expanding the resulting polynomials, cross terms yield exactly the desired multi-mode interaction. For example, $e^{i2\delta\hat{X}_j\hat{X}_k\hat{X}_l}$ is built by generating $(\hat{X}_j + \hat{X}_k + \hat{X}_l)^3$ and subtracting off the unwanted lower-order terms using additional cubic gates.

The universal gate set is $\{e^{i\frac{\pi}{2}(\hat{X}^2 + \hat{P}^2)},\; e^{it_1\hat{X}},\; e^{it_2\hat{X}^2},\; e^{it_3\hat{X}^3},\; e^{i\tau\hat{X}_1\hat{X}_2}\}$.

Gate counts: $e^{i2\delta\hat{X}_j\hat{X}_k\hat{X}_l}$ decomposes into 12 universal gates; $e^{i6\delta\hat{X}_j^2\hat{X}_k\hat{X}_l}$ into 873 (1749 when expanded to pure $\hat{X}$ operators via Fourier conjugation). These are finite, exact — no Trotter splitting, no error accumulation.

## When to reach for it

- CV Hamiltonian simulation of polynomial interactions (relevant for quantum optics, continuous-variable quantum computing)
- When the target Hamiltonian contains low-degree polynomial terms in position/momentum and you want exact circuits rather than Trotterisation
- The PDE solver of [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes|Arrazola et al. (2019)]], where the simulation Hamiltonian $\hat{A}\hat{X}\hat{Y}$ produces exactly these multi-mode polynomial terms

## Complexity

| Target unitary | Exact gate count | Commutator approx. ($\epsilon = 10^{-3}$) |
|---|---|---|
| $e^{i2\delta\hat{X}_j\hat{X}_k\hat{X}_l}$ | 12 | $\sim 10^6$ |
| $e^{i6\delta\hat{X}_j^2\hat{X}_k\hat{X}_l}$ | 873 | $\sim 2.8 \times 10^7$ |

The savings are dramatic: exact decomposition beats commutator approximation by 4–5 orders of magnitude.

## Caveat

- Only works for polynomial Hamiltonians in position/momentum. Non-polynomial interactions (e.g., Kerr $e^{i\hat{n}^2}$) still need approximation schemes.
- Requires the cubic phase gate $e^{it\hat{X}^3}$ as a primitive, which is non-Gaussian and experimentally demanding.
- Gate count grows with polynomial degree. The paper only works out trilinear and quadratic-linear cases explicitly. Higher-degree interactions produce combinatorial blowup in the decomposition.

## Related notes
- [[Quantum Algorithm for Nonhomogeneous Linear PDEs (Arrazola-Kalajdzievski-Weedbrook-Lloyd 2019) — Paper Notes]]
- [[Fourier Decomposition Technique for Operator Inversion]]
