
> **Tags:** #trick #QSP-precursor #GQSP #feasibility #polynomials
> **Source:** Low–Yoder–Chuang PRX 2016 (1603.03996); extended in Motlagh–Wiebe arXiv:2308.01501 (2023)

## What it does
Determines whether a desired composite-gate response is physically realizable before any phase synthesis.

## The trick
Express composite gate as
$$U(\theta)=A(x)I+iB(x)\sigma_z+iC(y)\sigma_x+iD(y)\sigma_y,$$
with $x=\cos(\theta/2), y=\sin(\theta/2)$.

Feasibility is equivalent to:
- degree bounds (≤ L)
- parity constraints by L
- boundary constraints (e.g. identity at zero angle)
- unitarity polynomial identity (sum-of-squares = 1)

This gives necessary and sufficient realizability conditions.

## When to use
Before optimizing/synthesizing any [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]]-like phase sequence.

## Extension: GQSP norm condition on the unit circle

[[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes|Motlagh–Wiebe (2023)]] gives the corresponding characterisation for **complex** polynomials on the unit circle: $P, Q \in \mathbb{C}[z]$ with $\deg \leq d$ are achievable by GQSP iff $|P(z)|^2 + |Q(z)|^2 = 1$ for all $|z| = 1$. No parity constraint, no realness constraint. The proof uses Fourier orthogonality — the norm condition on the unit circle equates Fourier coefficients of $|P|^2 + |Q|^2$ to $\delta_{k,0}$, and the inductive step exploits that cancelling the leading coefficient reduces the degree by 1.

## Caveat

For standard QSP (LYC): ignoring parity early is a common silent failure mode.

For GQSP: the condition $|P|^2 + |Q|^2 = 1$ on $\mathbb{T}$ is necessary and sufficient but doesn't directly hand you $Q$ — you still need [[FFT-Based Complementary Polynomial Optimisation]] or a Riesz-Fejér factorisation to compute $Q$ from $P$.

## Related notes

- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Generalized Quantum Signal Processing (Motlagh-Wiebe 2023) — Paper Notes]]
- [[Generalized Quantum Signal Processing (GQSP)]]
- [[FFT-Based Complementary Polynomial Optimisation]]
