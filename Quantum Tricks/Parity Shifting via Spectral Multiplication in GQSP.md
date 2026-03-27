
> **Source:** Berry, Motlagh, Pantaleoni, Wiebe, arXiv:2401.10321
> **Tags:** #trick #QSP #GQSP #parity #polynomials

## What it does

Shifts the Fourier parity of a polynomial from even to odd (or vice versa) by multiplying by $e^{i\theta}$ (equivalently, by a single power of $U$), then uses [[Generalized Quantum Signal Processing (GQSP)|GQSP]]'s ability to handle complex-valued polynomials to absorb the phase factor.

## The trick

When using [[Directional Walk Control for Phase Doubling|directional walk control]] ($U$ vs $U^\dagger$), the accessible Q-polynomial has only **even** Fourier components. But the [[Jacobi-Anger Truncation for QSP Simulation|Jacobi–Anger expansion]] of $\sin[\tau\sin\theta]$ has only **odd** components:

$$\sin[\tau\sin\theta] = 2\sum_{k\text{ odd}>0} J_k(\tau)\sin(k\theta).$$

Fix: multiply $Q(\theta)$ by $e^{i\theta}$. The product $e^{i\theta}\sin[\tau\sin\theta]$ now has even Fourier components — matching what the directional-control circuit can produce. But the result is complex.

Standard [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] requires real polynomials, so this would be a dead end there. [[Generalized Quantum Signal Processing (GQSP)|GQSP]] (Motlagh–Wiebe, arXiv:2308.01501) removes the real constraint, allowing $(P, Q) \in \mathbb{C}[z^{-1}, z]$ subject only to $|P|^2 + |Q|^2 = 1$ on the unit circle.

After the GQSP circuit implements $UP(\cdot)$, two additional controlled operations strip the extra factor of $U$.

## When to reach for it

- Any time directional or bidirectional control over a walk operator produces polynomial access with the wrong Fourier parity for your target function.
- Extending QSP algorithms that currently require definite parity (e.g., the even/odd split in [[Affine Spectrum Compression for QSP Parity Bypass|affine compression]]) to use complex GQSP instead.
- More generally, whenever a desired eigenvalue transformation has mixed parity and you want to avoid the overhead of splitting into even and odd parts and combining via [[Linear Combination of Unitaries (LCU)|LCU]].

## Complexity

One extra power of $U$ (absorbed into the GQSP circuit construction), plus two boundary controlled operations to remove it at the end. Negligible overhead.

## Caveat

Requires GQSP phase-finding algorithms. Standard QSP phase-finding methods (e.g., [[Efficient Phase-Factor Evaluation in QSP (Dong-Meng-Whaley-Lin 2021) — Paper Notes|QSPPACK]]) work with real polynomial constraints and would need modification. The GQSP phase-finding problem may be computationally harder in practice.

## Related notes

- [[Doubling the Efficiency of Hamiltonian Simulation via Generalized Quantum Signal Processing (Berry-Motlagh-Pantaleoni-Wiebe 2024) — Paper Notes]] — the paper that uses this trick
- [[Directional Walk Control for Phase Doubling]] — the companion trick that creates the parity mismatch
- [[Affine Spectrum Compression for QSP Parity Bypass]] — alternative parity-bypass strategy (stays within real QSP)
- [[Jacobi-Anger Truncation for QSP Simulation]] — the expansion whose parity needs fixing
- [[Polynomial Completion via Sum-of-Squares]] — the completion step that follows parity shifting
