# Review: A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022)

Source note: [[A Theory of Quantum Subspace Diagonalization (Epperly-Lin-Nakatsukasa 2022) — Paper Notes]]

## Primary Sources Checked

- Epperly, Lin, and Nakatsukasa, "A Theory of Quantum Subspace Diagonalization," SIAM Journal on Matrix Analysis and Applications 43(3), 1263-1290 (2022) / arXiv:2110.07492.

## Verdict

Minor issues. This is a technically strong note. The main improvement is to avoid summarizing QSD as simply "handles noise"; the theorem handles structured perturbations in measured matrices under thresholding assumptions.

## Line-Anchored Comments

- Lines 7-11: excellent opening. Add that thresholding does not make an arbitrary ill-conditioned generalized eigenproblem stable; it works because the QSD matrix pair has additional structure.
- Lines 20-32: the algorithm description is clear. The note assumes Hadamard-test access to overlaps; mention that concrete QSD implementations may use different overlap/transition-amplitude measurement circuits.
- Lines 34-42: the geometric-mean condition is the right central insight. Say whether it is proved for the QSD construction under stated assumptions or observed/parameterized for physical instances, since the later caveat says `alpha` is empirical.
- Lines 46-58: the perturbation-bound summary is good. Because the theorem is stated in eigenangle/arctangent form, add a sentence about converting this back to an energy error on a bounded spectral interval.
- Lines 60-74: the Krylov approximation bound is carefully written. The dependence on spectral width versus gap is important and should remain prominent.
- Lines 92-97: practical guidance is useful. The Toeplitz measurement reduction assumes exact time-translation structure; sampling noise, time-dependent simulation error, or hardware drift can spoil the exact Toeplitz model.
- Lines 99-109: the comparison table should change "Handles noise? Yes" to something like "matrix-measurement perturbations under thresholding." That is the actual theorem.
- Lines 111-117: the limitations are excellent. The Hamiltonian-simulation primitive caveat should be moved earlier because it is a major resource in any end-to-end algorithm.

## Missing or Suggested Additions

- Add one sentence distinguishing QSD's measured-matrix noise model from physical circuit noise. The former can include the latter after reduction to `Delta H, Delta S`, but only if biases and simulation errors are controlled.

