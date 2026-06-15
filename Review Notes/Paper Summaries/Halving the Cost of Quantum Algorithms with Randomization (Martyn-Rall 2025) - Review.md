# Review: Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025)

Source note: [[Halving the Cost of Quantum Algorithms with Randomization (Martyn-Rall 2025) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2409.03744
- npj Quantum Information: https://www.nature.com/articles/s41534-025-01003-2

## Verdict

Minor issues, with one metadata correction. The note captures stochastic QSP well, especially the channel-versus-coherent-unitary caveat. Fix the npj article number and one ratio formula, and tighten the scope of the "halving" claim.

## Line-Anchored Comments

- Line 1: The npj Quantum Information article number is `47`, not `51`, according to the journal page and arXiv metadata. The paper is npj Quantum Information 11, 47 (2025), published March 15, 2025.

- Lines 9-12: The target channel description is good. Add that this is a randomized channel implementation of a polynomial transformation; it is not a coherent block-encoding of `F(A)` suitable for arbitrary amplitude amplification.

- Lines 17-21: The source abstract supports the broad QSP-family scope, but "nearly all known quantum algorithms" should be softened to "nearly all QSP-based algorithms" or "many algorithms expressible through QSP/QSVT."

- Lines 31-44: The mechanism is stated well. The key is not that every sampled polynomial is accurate, but that each is only `O(sqrt(epsilon))` accurate while the average is `O(epsilon)` and the mixing lemma converts this into channel error.

- Lines 46-50: The displayed ratio is wrong or at least mislabeled. If `d_avg <= d/2 + O(1)`, then `d_avg/d -> 1/2`; `2 d_avg/d` instead tends to `1`, not `1/2`.

- Lines 97-104: The application table should distinguish functions whose degree has a multiplicative `log(1/epsilon)` precision factor from real-time Hamiltonian simulation, where the precision term is often `log(1/epsilon)/loglog(1/epsilon)` plus the time term. The halving applies to the precision-dependent degree component, not the entire algorithm cost.

- Lines 137-147: The limitations section is strong. Emphasize that classical random sampling of the circuit branch must be outside any coherent superposition; otherwise the method no longer implements the intended mixture.

## Missing or Suggested Additions

- Add the arXiv version line: as of the checked page, arXiv:2409.03744 is at v4, March 25, 2025, with the npj reference attached.

- Add an explicit "not a black-box factor-two speedup for all workloads" caveat: if state preparation, block-encoding construction, measurements, or the time-dependent part dominates, the end-to-end improvement can be much smaller.

