# Symmetry-Based QSE for Unencoded Error Mitigation

> **Source:** McClean, Jiang, Rubin, Babbush, Neven, arXiv:1903.05786
> **Tags:** #trick #error-mitigation #subspace-expansion #symmetry-verification #NISQ #quantum-chemistry

## What it does

Applies the quantum subspace expansion (QSE) framework to known symmetries of a physical Hamiltonian — without any error-correcting code — to mitigate noise via post-processed projection.

## The trick

For an unencoded Hamiltonian $H_p$ with known $Z_2$ symmetries $\{F_1, \ldots, F_k\}$ (operators with eigenvalues $\pm 1$ that commute with $H_p$), use them as "stabilizer generators" in the same projection framework used for QEC codes.

**Exact symmetries:** Particle number parity ($\prod_i Z_i$ in Jordan-Wigner), spin-up parity ($\prod_{i \in \alpha} Z_i$), spin-down parity ($\prod_{i \in \beta} Z_i$). These are guaranteed to commute with the Hamiltonian and the correct eigenvalue is known from the problem specification.

**Approximate symmetries:** Local occupation operators $Z_i$ don't commute with $H_p$ in general, but for high-energy orbitals, the occupation is approximately fixed. The QSE generalized eigenvalue problem:

$$HC = SCE, \quad H_{ij} = \text{Tr}(M_i^\dagger H_p M_j \rho), \quad S_{ij} = \text{Tr}(M_i^\dagger M_j \rho)$$

automatically decides the optimal coefficient for each projector, balancing the cost of discarding correct wavefunction amplitude against the noise reduction from removing erroneously occupied orbitals.

**Automatic symmetry sector selection:** If you know $[H_p, F] = 0$ but don't know which eigenspace ($+1$ or $-1$) contains the ground state, include both $(I+F)$ and $(I-F)$ as expansion operators. The QSE will select the correct sector without you having to specify it.

**Pair projectors:** $(I + Z_i Z_j)$ for pairs of high-energy orbitals removes correlated double-excitation errors that single-qubit projectors miss.

## When to reach for it

- Any [[Variational Quantum Eigensolver (VQE)|VQE]] or variational calculation on a physical (unencoded) Hamiltonian where you know symmetries of the target state.
- The $Z_2$ parity projections are essentially free (small constant number of extra Pauli measurements) and always help — there's no pseudo-threshold to cross.
- You suspect the noise is exciting high-energy modes (common in depolarizing noise models) and want to project those excitations out.
- As a complement to [[Number-Basis Postselection for Error Mitigation|number-basis postselection]] — this generalizes beyond particle-number symmetry to arbitrary symmetries and non-commuting approximate symmetries.

## Complexity

- **Additional qubits / depth:** Zero. Everything is post-processing.
- **Measurements:** For $k$ exact $Z_2$ symmetries, $2^k$ stabilizer group elements (manageable for small $k$). The [[Stochastic Stabilizer Sampling for Post-Processed QEC|stochastic sampling]] scheme makes cost depend on state quality, not $k$.
- **Classical post-processing:** Solve $k \times k$ generalized eigenvalue problem (trivial).
- **Improvement:** Up to $\sim 3\times$ reduction in logical infidelity demonstrated on H₂ under depolarizing noise.

## Caveat

- Only catches errors that violate the symmetries you know about. Errors within the correct symmetry sector pass through.
- The approximate symmetry approach (local $Z_i$ projectors) can backfire if the true ground state has non-negligible occupation of the orbital you're projecting out. The QSE helps here, but the decision depends on the accuracy of $\rho$.
- For molecules with many approximate symmetries, the QSE matrix grows and the measurement cost for its elements accumulates. In practice, you'd want to select only the most useful symmetries (those protecting against the most damaging errors).
- Combines naturally with zero-noise extrapolation — extrapolate each QSE matrix element, then solve — but this hasn't been demonstrated numerically.

## Related notes
- [[Decoding Quantum Errors with Subspace Expansions (McClean, Jiang, Rubin, Babbush, Neven 2019) — Paper Notes]]
- [[Stabilizer Subspace Projection for Error Mitigation]] — the encoded version of this trick
- [[Number-Basis Postselection for Error Mitigation]] — simpler symmetry-based mitigation via particle counting
- [[Symmetry Reduction in Qubit Hamiltonians]] — same symmetries used for qubit reduction rather than error mitigation
- [[Virtual Quantum Subspace Expansion (VQSE)]] — same QSE framework applied to basis extension
- [[Variational Quantum Eigensolver (VQE)]]
- [[2-RDM Physicality Restoration by PSD Projection]] — alternative post-processing error mitigation
