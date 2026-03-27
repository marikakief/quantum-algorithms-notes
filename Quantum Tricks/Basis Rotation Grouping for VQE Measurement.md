
> **Source:** Huggins, McClean, Rubin, Jiang, Wiebe, Whaley, Babbush, arXiv:1907.13117
> **Tags:** #trick #measurement #VQE #NISQ #quantum-chemistry #low-rank #diagonal-measurement

## What it does

Reduces the number of distinct measurement circuits needed for [[Variational Quantum Eigensolver (VQE)|VQE]] energy estimation from $O(N^4)$ (naive Pauli decomposition) or $O(N^3)$ (best Pauli grouping) to $O(N)$, for molecular Hamiltonians with $N$ spin-orbitals. Also reduces the total shot count by up to three orders of magnitude for modest system sizes, with empirically fitted scaling $\sim N^{2.75}$.

## The trick

Use the [[Double Factorization of Two-Electron Integrals|eigendecomposition of the two-electron integral tensor]] to write the Hamiltonian as:

$$H = U_0 \left(\sum_p g_p\, n_p\right) U_0^\dagger + \sum_{\ell=1}^{L} U_\ell \left(\sum_{pq} g^{(\ell)}_{pq}\, n_p n_q\right) U_\ell^\dagger$$

where $n_p = a_p^\dagger a_p$ and each $U_\ell$ is a single-particle basis rotation. For molecular Hamiltonians, $L = O(N)$.

**Measurement protocol:** For each factor $\ell$:
1. Prepare $|\psi(\theta)\rangle$.
2. Apply $U_\ell^\dagger$ via a [[Givens Rotation Slater Determinant Preparation|Givens rotation network]]: $N^2/4 - N/2$ two-qubit gates, depth $N/2$, linear connectivity.
3. Measure all qubits in the computational basis.
4. From bitstrings, extract all $\langle n_p \rangle_\ell$ and $\langle n_p n_q \rangle_\ell$ simultaneously (they're all diagonal).

The energy is reconstructed as $\langle H \rangle = \sum_p g_p \langle n_p \rangle_0 + \sum_\ell \sum_{pq} g^{(\ell)}_{pq} \langle n_p n_q \rangle_\ell$.

Because all measurements are diagonal, each shot also measures total particle number $\eta$ and $S_z$, enabling [[Number-Basis Postselection for Error Mitigation|postselection on symmetry eigenvalues]] for free.

Shots are distributed across the $L+1$ groups using optimal allocation (approximate variances from a CISD calculation suffice, with $< 3\%$ overhead).

## When to reach for it

- Any [[Variational Quantum Eigensolver (VQE)|VQE]] or variational algorithm measuring a molecular Hamiltonian in an arbitrary Gaussian basis.
- When measurement shot count is the practical bottleneck (which it usually is for VQE beyond toy systems).
- When hardware has linear connectivity — the Givens rotation circuits require only nearest-neighbour gates.
- Generalizes the [[Variational Measurement Reduction via Dual Basis]] trick from the plane-wave dual basis ($L = 1$) to arbitrary molecular orbital bases ($L = O(N)$).
- Especially effective for larger basis sets and extended systems approaching the thermodynamic limit.

## Complexity

- **Distinct circuits:** $L + 1 = O(N)$
- **Gates per circuit:** $N^2/4 - N/2$ two-qubit (Givens rotations), depth $N/2$, linear connectivity
- **Total shots (empirical):** $\sim N^{2.75}/\varepsilon^2$ (fitted on H-chains); vs $\sim N^{4.9}/\varepsilon^2$ for Pauli grouping
- **Classical preprocessing:** Eigendecomposition of the two-electron integral tensor ($O(N^5)$ naively, but this is standard in classical quantum chemistry and usually takes negligible time)

## Caveat

- Adds $O(N^2)$ gates per measurement circuit. On very noisy hardware, the added noise from the Givens rotation may outweigh the measurement savings. Simulations show the tradeoff is favorable for two-qubit gate error rates $\lesssim 10^{-2}$.
- The $N^{2.75}$ exponent is an empirical fit to H-chains, not a proven bound. Different molecular geometries may show different scaling.
- For small systems in minimal basis sets (e.g., H₂O / STO-3G), may not improve over simpler Pauli grouping.
- The favourable covariance structure that drives the shot count reduction below the group-count estimate is system-dependent and not analytically guaranteed.

## Related notes
- [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]]
- [[Double Factorization of Two-Electron Integrals]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[Variational Measurement Reduction via Dual Basis]]
- [[Variance Reduction via N-Representability Constraints (LP Method)]]
- [[Pauli Expectation Value Estimation]]
- [[Number-Basis Postselection for Error Mitigation]]
- [[Partial Basis Rotation (Reduced Givens Count)]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]] — Fault-tolerant alternative achieving $\tilde{O}(\sqrt{M}/\varepsilon)$ queries in the high-precision regime; sampling-based methods like this one are preferred when $\varepsilon > 1/\sqrt{M}$
