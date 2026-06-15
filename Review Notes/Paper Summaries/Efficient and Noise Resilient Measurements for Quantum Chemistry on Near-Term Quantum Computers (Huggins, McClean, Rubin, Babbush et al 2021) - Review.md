# Review: Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins-McClean-Rubin-Babbush et al. 2021)

Source note: [[Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers (Huggins, McClean, Rubin, Babbush et al 2021) — Paper Notes]]

## Primary Sources Checked

- Huggins et al., "Efficient and Noise Resilient Measurements for Quantum Chemistry on Near-Term Quantum Computers," npj Quantum Information 7, 23 (2021) / arXiv:1907.13117.

## Verdict

Minor issues. The note is accurate and captures why basis-rotation grouping mattered. It mainly needs sharper separation between fewer measurement circuits, lower true variance, and lower wall-clock cost on noisy hardware.

## Line-Anchored Comments

- Lines 15-19: the headline claims are fair, but "quartic reduction" is a group-count statement. The note should continue to emphasize that sample complexity depends on variances and covariances, not only on the number of measurement settings.
- Lines 23-39: the factorized Hamiltonian presentation is good. The statement that `L = O(N)` for arbitrary basis molecular Hamiltonians should be softened; it is empirically favorable and basis/threshold dependent, not a worst-case theorem for all integral tensors.
- Lines 41-52: clear measurement protocol. Add that the Givens basis rotations are extra circuit depth before every shot, so the benefit depends on the hardware error regime.
- Lines 54-56: good inclusion of approximate shot allocation from CISD. Say that this is an empirical/practical allocation method, not a requirement of the estimator.
- Lines 58-62: "zero additional circuit cost" is true for extracting particle number and `S_z` from the same bitstrings, but postselection has a sample overhead from discarded shots. Add that explicitly.
- Lines 64-66: the readout-error argument is useful but model-dependent: the `(1-2p)^K` suppression assumes an independent symmetric readout bitflip model and does not include the additional basis-rotation gate errors.
- Lines 76-88: the numerical results are well summarized. Make the H-chain exponent visibly empirical and limited to the studied systems.
- Lines 104-116: the caveats are strong. The rank and covariance caveats should be moved earlier or cross-referenced from the headline claim.

## Missing or Suggested Additions

- Add a compact resource accounting table with three rows: number of settings, two-qubit gates per setting, and estimated shots. That would stop readers from equating `O(N)` settings with `O(N)` total cost.

