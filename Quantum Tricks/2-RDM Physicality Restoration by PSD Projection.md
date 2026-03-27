
> **Source:** Rubin, Babbush, McClean, arXiv:1801.03524, New J. Phys. 20, 053020 (2018)
> **Tags:** #trick #error-mitigation #RDM #n-representability #noise #quantum-chemistry #PSD-projection

## What it does

Restores the physical validity of a 2-body reduced density matrix (2-RDM) that has been corrupted by hardware noise, by projecting it back onto the cone of approximately $n$-representable matrices. Useful for near-term quantum chemistry experiments where noise shifts the measured state outside the set of valid fermionic states.

## The trick

A valid $n$-electron fermionic 2-RDM must satisfy the **DQG conditions**: the 2D, 2Q, and 2G matrices must all be positive semidefinite (PSD) with fixed traces. Hardware noise breaks these conditions — measured RDMs often have negative eigenvalues and wrong traces.

**Three levels of projection, in order of computational cost:**

### Level 1: Simple PSD projection

Given a noisy measured ${}^2D_\text{meas}$:
1. Hermitize: ${}^2D_s = ({}^2D_\text{meas} + {}^2D_\text{meas}^\dagger)/2$
2. Eigendecompose: ${}^2D_s = U \text{diag}(\lambda_i) U^\dagger$
3. Clip negative eigenvalues: $\lambda_i' = \max(\lambda_i + t, 0)$ where $t$ is chosen so $\sum_i \lambda_i' = n(n-1)$
4. Reconstruct: ${}^2D_\text{proj} = U \text{diag}(\lambda_i') U^\dagger$

This minimizes $\|{}^2D_\text{proj} - {}^2D_\text{meas}\|_F$ subject to ${}^2D_\text{proj} \succeq 0$ and correct trace. Cost: $O(m^6)$ for $m$ spin-orbitals (diagonalize an $\binom{m}{2} \times \binom{m}{2}$ matrix). Fast for small $m$.

### Level 2: Iterative DQG projection (recommended heuristic)

Exploits all three DQG conditions by alternating projections:
1. Hermitize and PSD-project ${}^2D$ to trace $n(n-1)$
2. Map ${}^2D \to {}^2Q$ via particle-hole exchange: ${}^2Q^{ij}_{kl} = \delta^{il}\delta^{jk} - \delta^{il}{}^1D^j_k - \delta^{jk}{}^1D^i_l + {}^2D^{ij}_{kl} + \ldots$ (full anticommutation map)
3. PSD-project ${}^2Q$ to trace $(m-n)(m-n-1)$
4. Map ${}^2Q \to {}^2G$ (mixed particle-hole)
5. PSD-project ${}^2G$ to trace $n(m-n+1)$
6. Contract back to ${}^2D$; repeat from step 1 until convergence

**Convergence criterion:** max negative eigenvalue across all three matrices $< 10^{-7}$. Typically converges in $\sim 10$–$100$ iterations. Much cheaper than an SDP; no theoretical convergence guarantee but works well in practice for moderate noise.

### Level 3: SDP reconstruction (exact)

For maximum fidelity, formulate as an SDP:

$$\min_{{}^2D_\text{recon}} \|{}^2D_\text{recon} - {}^2D_\text{meas}\|_F^2 \quad \text{s.t.} \quad {}^2D, {}^2Q, {}^2G \succeq 0, \text{ trace constraints, linear maps}$$

The Frobenius objective is cast as a semidefinite constraint:
$$\begin{pmatrix} I & E \\ E^\dagger & F \end{pmatrix} \succeq 0, \quad E = {}^2D_\text{recon} - {}^2D_\text{meas}$$
then minimize $\text{Tr}[F]$ as a proxy for $\|E\|_F^2$. Polynomial-time but high practical cost; suited for small systems ($m \lesssim 10$) or when exact representability is needed.

**Demonstration:** Rubin et al. apply all three methods to 4-qubit H₂ under dephasing, amplitude damping, and depolarizing noise. The iterative DQG projection correctly restores the H₂ energy curve across the range of bond lengths where the bare measured curve is visibly distorted.

## When to reach for it

- Any near-term VQE or quantum chemistry experiment where partial state tomography (measuring the 2-RDM) is corrupted by hardware errors.
- Situation: you've measured enough Pauli terms to reconstruct the 2-RDM, but the result has negative eigenvalues or wrong trace — physical states can't do this.
- Post-processing step in [[Variational Quantum Eigensolver (VQE)|VQE]]: project before computing energy from RDM.
- Also useful for error mitigation in [[Pauli Expectation Value Estimation|Pauli expectation value]] estimation: if variance in individual measurements makes the assembled 2-RDM nonphysical, project first.

## Complexity

- **Simple PSD projection:** $O(m^6)$ (diagonalize $m^2 \times m^2$ matrix).
- **Iterative DQG:** $O(K \cdot m^6)$ for $K$ iterations; $K$ typically $< 100$; still $O(m^6)$ overall.
- **SDP:** Polynomial in $m^4$ but with large constants; practical only for $m \lesssim 10$.
- **Measurement cost of assembling 2-RDM:** $O(m^4)$ Pauli terms must be measured. This is the dominant cost; projection is cheap by comparison.

## Caveat

- Projection finds the nearest $n$-representable matrix by Frobenius distance — not necessarily the one closest to the actual quantum state. If noise is large and structured, the projected result may be physically valid but wrong.
- The DQG conditions are necessary but not sufficient. The projected matrix satisfies DQG but may still not be exactly $n$-representable (full necessary and sufficient conditions are NP-hard to enforce).
- Iterative projection has no convergence guarantee in general. It works when the measured 2-RDM is "close" to the physical set. For very high error rates, it may oscillate or converge to a wrong fixed point.
- Requires knowing $n$ (electron number) and $m$ (orbital count) to set the correct trace constraints.

## Related notes
- [[Application of Fermionic Marginal Constraints to Hybrid Quantum Algorithms (Rubin, Babbush, McClean 2018) — Paper Notes]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Pauli Expectation Value Estimation]]
- [[Variance Reduction via N-Representability Constraints (LP Method)]]
- [[Symmetry Reduction in Qubit Hamiltonians]]
