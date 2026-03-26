# Generalized Quantum Signal Processing (GQSP)

> **Source:** Motlagh and Wiebe, arXiv:2308.01501 (2023)
> **Tags:** #trick #QSP #GQSP #polynomials #unitary-transformation

## What it does

Extends [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|quantum signal processing]] to implement **complex** polynomial transformations of a unitary $U$, removing the restriction to real-valued polynomial pairs.

## The trick

Standard QSP (Low–Chuang, [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al.]]) implements polynomial transformations where the matrix entries $A(\theta), B(\theta), C(\theta), D(\theta)$ are **real** functions. This means the accessible transformations must have definite parity and satisfy real-coefficient constraints.

GQSP replaces this with: given a unitary $U$, a sequence of $d$ controlled-$U$ operations interspersed with parameterized SU(2) rotations implements

$$\begin{pmatrix} P(U) & \cdot \\ Q(U) & \cdot \end{pmatrix}$$

where $P, Q \in \mathbb{C}[z]$ with $\deg \leq d$ and $|P(z)|^2 + |Q(z)|^2 = 1$ on $|z| = 1$.

The key relaxation: $P$ and $Q$ can be **complex** polynomials. This opens up transformations that standard QSP cannot reach, including those with mixed Fourier parity.

## When to reach for it

- When the target eigenvalue transformation is complex-valued (e.g., $e^{-i\tau\sin\theta}$ rather than $\cos(\tau\sin\theta)$ alone).
- When [[Directional Walk Control for Phase Doubling|directional walk control]] produces polynomials with the wrong Fourier parity, and [[Parity Shifting via Spectral Multiplication in GQSP|spectral multiplication]] shifts them at the cost of making the polynomial complex.
- Any setting where standard QSP's real-polynomial constraint forces you to split into even/odd parts and recombine via [[Linear Combination of Unitaries (LCU)|LCU]] — GQSP may allow a single-pass construction instead.

## Complexity

Same query complexity as standard QSP: $d$ controlled operations for a degree-$d$ polynomial. The classical cost of computing the rotation angles may be higher than for standard QSP phase-finding.

## Caveat

When both $P$ and $Q$ are known, the recursive angle extraction (Algorithm 1 in Motlagh-Wiebe) is exact and simple — peel off leading coefficients one at a time. When only $P$ is known, you need to find $Q$ first; [[FFT-Based Complementary Polynomial Optimisation]] handles this via gradient descent on the autocorrelation norm condition, scaling to degree $\sim 10^7$ on GPU. Neither step has the stability guarantees of [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes|QSPPACK]] at modest degrees, but empirically both work well for practical polynomial degrees.

## Related notes

- [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes]] — source paper
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]] — the standard (real-polynomial) QSP this generalises
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]] — QSVT framework (real polynomials)
- [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes]] — first application of GQSP to Hamiltonian simulation
- [[Directional Walk Control for Phase Doubling]] — the trick that necessitates GQSP
- [[Parity Shifting via Spectral Multiplication in GQSP]] — companion technique
- [[FFT-Based Complementary Polynomial Optimisation]] — finding $Q$ from $P$ to enable angle extraction
- [[Recursive Phase Extraction from Polynomial Coefficients]] — extracting rotation angles from $(P, Q)$
- [[SU(2) Response Tuple Characterization]] — the norm condition $|P|^2 + |Q|^2 = 1$ that characterises valid GQSP polynomial pairs
