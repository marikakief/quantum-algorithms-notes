# Review: Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025)

Source note: [[Fast Quantum Simulation of Electronic Structure by Spectrum Amplification (Low, King, Berry, Babbush, Somma, Rubin 2025) — Paper Notes]]

## Primary Sources Checked

- Low et al., "Fast quantum simulation of electronic structure by spectrum amplification", arXiv:2502.15882 / Physical Review X 15, 041016 (2025).

## Verdict

Major issues. The core idea is captured well, but the note contains at least one large numerical overclaim and should be more careful about sum-of-squares existence versus efficient chemistry-structured decompositions.

## Line-Anchored Comments

- Lines 19-23: the `sqrt(2 Lambda E_gap)` story matches the paper's headline. Define `E_gap` as the lowest energy of the sum-of-squares shifted Hamiltonian, not the physical spectral gap.
- Line 35: "Any Hamiltonian can be written as SOS" needs qualification. A finite-dimensional lower-bounded Hamiltonian can be shifted to positive semidefinite form and factored abstractly, but an efficient chemistry-structured SOS decomposition is the substantive claim.
- Lines 37-43: the norm definitions are mostly right. Make explicit which symbol denotes the original block-encoding normalization and which denotes the SOS/square-root normalization.
- Line 57: empirical fits for factorization ranks should be labeled empirical. DFTHC and BLISS-style optimizations do not provide universal rank laws for arbitrary molecules.
- Lines 94-99: the cost discussion should distinguish the spectrum-amplified walk, the underlying rectangular block encoding, and the outer phase-estimation/search routine.
- Line 198: the "300,000x" improvement over Reiher appears numerically wrong if comparing `5e13` to roughly `1e9`: that is about `5e4`, not `3e5`. If a different baseline is intended, name the baseline and numbers explicitly.
- Line 213: the SOSSA reference is useful, but make clear whether it is the general algorithmic follow-up or the chemistry-specific PRX implementation.

## Missing or Suggested Additions

- Add a short warning that "spectrum amplification" improves the dependence on the normalization only when the SOS lower-bound gap is favorable and the SOS block encoding is not too costly.

