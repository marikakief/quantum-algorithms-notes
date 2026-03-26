# Force Operator Block Encoding via Hamiltonian Differentiation

> **Source:** O'Brien, Streif, Rubin, Santagati, Su, Huggins et al., arXiv:2111.12437
> **Tags:** #trick #block-encoding #quantum-chemistry #force-estimation #fault-tolerant

## What it does

Produces a block encoding of the force (energy derivative) operator $dH/dR_i$ by differentiating the factorised form of the Hamiltonian, reusing the same block-encoding machinery with at most constant-factor overhead.

## The trick

Given a factorised Hamiltonian $V = \sum_l W^{(l)} W^{(l)\dagger}$ (e.g., [[Double Factorization of Two-Electron Integrals|double factorisation]] or [[THC Non-Orthogonal Diagonal Coulomb Representation|THC]]), the derivative is:

$$\frac{dV}{dR_i} = \sum_l \frac{dW^{(l)}}{dR_i} W^{(l)\dagger} + W^{(l)} \frac{dW^{(l)\dagger}}{dR_i}$$

Rewrite as a difference of squares:

$$\frac{dV}{dR_i} = \frac{1}{2}\sum_l \left[\left(W^{(l)} + \frac{dW^{(l)}}{dR_i}\right)^2 - \left(W^{(l)} - \frac{dW^{(l)}}{dR_i}\right)^2\right]$$

Each squared term has the same structure as the original Hamiltonian factors — a one-body operator squared, diagonalisable by a Givens rotation. This means the same [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]] circuit applies with:
- One extra ancilla qubit to flag between the $+$ and $-$ terms
- Modified eigenvalues $f_p^{(l\pm)}$ from diagonalising $W^{(l)} \pm dW^{(l)}/dR_i$

For **THC**: differentiate the THC tensors $\chi, \zeta$ directly. The result has five terms (vs. one for the Hamiltonian) with tensors $\chi, \chi^{(+)}, \chi^{(-)}, \zeta, d\zeta/dR_i$. The critical diagonal structure is preserved because creation and annihilation operators always transform with the *same* tensor within each term. Circuit cost: $T_F \sim \sqrt{2}\, T_H$ (from the $\sqrt{2}\times$ larger QROM). Rescaling factor: $\lambda_{F_i} = \sum_{\mu\nu}(|\zeta_{\mu\nu}| + \frac{1}{2}|d\zeta_{\mu\nu}/dR_i|)$.

For **low-rank factorisation**: $T_F \sim O(\sqrt{NL})$ (same as Hamiltonian). Rescaling: $\lambda_{F_i} = \frac{1}{2}\sum_l [(\sum_p |f_p^{(l+)}|)^2 + (\sum_p |f_p^{(l-)}|)^2]$, worst-case $\sim 4\lambda_H$.

## When to reach for it

- Any fault-tolerant quantum chemistry calculation that needs energy derivatives: molecular forces, dipole moments, NMR coupling constants, or any property expressible as $dE/dx = \langle\psi|dH/dx|\psi\rangle$.
- When you already have a block encoding of $H$ via factorisation methods. The derivative inherits the same structure, so no new circuit architecture is needed.
- Especially attractive when $\lambda_F \ll \lambda_H$ (empirically true for local derivative operators in extended systems), since the FT algorithm cost scales with $\lambda_F$.

## Complexity

- **Circuit overhead:** At most constant factor ($\sqrt{2}\times$ for THC, $1\times$ for DF) over the Hamiltonian block encoding
- **Rescaling factor:** $\lambda_F \leq 4\lambda_H$ worst-case for DF; empirically much smaller ($\lambda_F^{(\text{DF})} \sim O(N_H^{0.06})$ vs. $\lambda_H^{(\text{DF})} \sim O(N_H^{1.94})$ on H-chains)

## Caveat

Differentiating the factorised Hamiltonian (rather than directly factorising $dH/dR_i$) guarantees consistency — the estimated force matches the energy manifold of the factorised Hamiltonian exactly. Direct factorisation of $dH/dR_i$ would likely give smaller $\lambda_F$ but introduces a systematic bias from the mismatch between truncation errors. The practical significance of this bias depends on the THC/DF truncation threshold and needs separate analysis.

Also, in second-quantised atomic-centred bases, the force operator includes **Pulay force** terms $\propto dS/dR_A$ that arise from the nuclear-position-dependent overlap matrix. These add a non-trivial one-body correction that has no analogue in the Hamiltonian block encoding.

## Related notes
- [[Efficient Quantum Computation of Molecular Forces and Other Energy Gradients (O'Brien, Babbush et al 2022) — Paper Notes]]
- [[Double Factorization of Two-Electron Integrals]]
- [[THC Non-Orthogonal Diagonal Coulomb Representation]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
- [[Even More Efficient Quantum Computations of Chemistry Through Tensor Hypercontraction (Lee, Berry, Babbush et al 2021) — Paper Notes]]
- [[Qubitization of Arbitrary Basis Quantum Chemistry Leveraging Sparsity and Low Rank Factorization (Berry, Gidney, Motta, McClean, Babbush 2019) — Paper Notes]]
- [[Single Factorization for Qubitized Chemistry]]
