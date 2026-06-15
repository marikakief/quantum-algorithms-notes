# Review: Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254)

Source note: [[Time-Dependent Hamiltonian Simulation with L1-Norm Scaling (Quantum 2020-04-20-254) — Paper Notes]]

## Primary Sources Checked

- Berry, Childs, Su, Wang, and Wiebe, "Time-dependent Hamiltonian simulation with L1-norm scaling", Quantum 2020 / arXiv:1906.07115.

## Verdict

Minor issues. The note clearly explains the time-reparameterization idea and the distinction between continuous qDRIFT and rescaled Dyson. The main issues are notation consistency and a few overstatements.

## Line-Anchored Comments

- Lines 15-19: good problem statement. Distinguish unitary-operator spectral-norm error from channel/diamond-norm error for randomized qDRIFT.
- Lines 25-35: good summary of the integrated-norm replacement.
- Lines 56-74: the rescaling principle is explained well. Mention earlier that an efficiently computable upper bound can replace the exact norm, because exact norm oracles are a strong assumption.
- Lines 80-108: theorem summaries are useful. Explain why Theorem 4 has `d^2` while Theorem 10 has `d`; otherwise the two sparse-model query statements look inconsistent.
- Lines 110-116: possible notation inconsistency. Earlier the LCU integrated coefficient norm is `||alpha||_{1,1}`, but Theorem 11 is written with `||alpha||_{\infty,1}`. Check the paper's notation and use one convention with definitions.
- Line 128: "strict improvement" should be softened. The `L^1` parameter is never larger than the `L^\infty`-times-time parameter, but it is a strict improvement only when the norm varies substantially.
- Lines 138-142: good caveats. The zero-crossing/differentiability issue should point back to using a positive upper-bound function `Lambda(t)` when exact normalization fails.
- Lines 152-159: references/cross-links are helpful; keep years consistent with the source notes.

## Missing or Suggested Additions

- Include the sampling distribution and channel nature of continuous qDRIFT in the key-results area, not only in the caveats.
