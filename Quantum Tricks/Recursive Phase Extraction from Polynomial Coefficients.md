
> **Tags:** #trick #phase-synthesis #QSP-precursor #GQSP #compiler
> **Source:** Low–Yoder–Chuang PRX 2016; Algorithm 1 in Motlagh–Wiebe arXiv:2308.01501 (2023)

## What it does
Compiles realizable polynomial response functions into a shortest sequence of phase angles.

## The trick
Given degree-L realizable tuple, recursively peel off one primitive rotation at a time:
1. Compute phase-sum coefficient representation.
2. Choose terminal phase to cancel highest-order component in residual.
3. Reduce to degree L-1 instance.
4. Repeat until base case.

Outputs exactly L phases (minimal length).

## Why it's important
Replaces exponential search over phase space with polynomial-time deterministic compilation.

## When to use
Any [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]/composite-pulse pipeline after polynomial design/completion.

## Extension: GQSP recursive angle extraction (Algorithm 1, Motlagh-Wiebe)

The same structure applies to [[Generalized Quantum Signal Processing (GQSP)|GQSP]]. Given $(P, Q)$ satisfying $|P|^2 + |Q|^2 = 1$ on $\mathbb{T}$ with $\deg P, \deg Q \leq d$:

1. Let $a_d$ = leading coefficient of $P$, $b_d$ = leading coefficient of $Q$.
2. Set $\theta_d = \arctan(|b_d|/|a_d|)$, $\phi_d = \arg(a_d/b_d)$.
3. Apply the inverse of the final SU(2) rotation to $(P, Q)$ to get a new pair $(\hat{P}, \hat{Q})$ of degree $\leq d-1$.
4. Recurse until degree 0; the base case gives $(\theta_0, \phi_0, \lambda)$.

This is exact (not an approximation) and $O(d^2)$ total if done naively, or $O(d \log d)$ with FFT-accelerated polynomial arithmetic. Requires knowing $Q$ upfront; combine with [[FFT-Based Complementary Polynomial Optimisation]] when only $P$ is known.

## Caveat

Stable numerics require careful coefficient conditioning for high degree. The LYC method breaks at degree $\gtrsim 10^4$; [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes|QSPPACK]] handles that for standard QSP. For GQSP the stability at very high degree ($\sim 10^7$) hasn't been fully analysed.

## Related notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes]]
- [[Generalized Quantum Signal Processing (GQSP)]]
- [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes]] — the optimisation-based replacement when this method breaks at high degree
- [[Energy Landscape of Symmetric QSP (Wang-Dong-Lin 2022) — Paper Notes]]
- [[FFT-Based Complementary Polynomial Optimisation]] — getting $Q$ from $P$ before angle extraction
