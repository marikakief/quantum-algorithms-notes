# Review: Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022)

Source note: [[Nearly Optimal Quantum Algorithm for Estimating Multiple Expectation Values (Huggins, Wan, McClean, Babbush et al 2022) — Paper Notes]]

## Primary Sources Checked

- Huggins et al., "Nearly optimal quantum algorithm for estimating multiple expectation values", arXiv:2111.09283 / Physical Review Letters 129, 240501 (2022).

## Verdict

Minor issues. The gradient-encoding idea is explained well. The query-versus-gate-cost caveat should be moved closer to the headline.

## Line-Anchored Comments

- Lines 15-17: the assessment is right: the result is about state-preparation oracle queries, and practical competitiveness depends on controlled evolutions and ancilla cost.
- Lines 23-35: the gradient identity is clear and appears correct.
- Lines 39-48: "each call uses exactly one query to `U_psi`" is too terse. The full gradient algorithm uses the probability oracle and its inverse; the resource statement should match the paper's accounting in calls to `U_psi` and `U_psi^\dagger`.
- Lines 76-85: the theorem summaries are good.
- Lines 90-97: the comparison table should not visually hide that the gradient method's saving is in state-preparation queries, not necessarily total controlled-observable evolution.
- Lines 99-109: the RDM and dynamic-correlation applications are useful. Check the evenly spaced time-point comparison; the units in the displayed `Delta` scaling look easy to miscopy.
- Lines 111-121: excellent caveats. Move line 117 into the first summary paragraph.

## Missing or Suggested Additions

- Add a "resource split" sentence: state-preparation queries, controlled `e^{-ixO_j}` cost, extra qubits, and classical post-processing are separate metrics.

