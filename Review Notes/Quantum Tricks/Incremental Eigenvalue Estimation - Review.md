# Review: Incremental Eigenvalue Estimation

Source note: [[Incremental Eigenvalue Estimation]]

## Primary Sources Checked

- Krovi, Magniez, Ozols, Roland, arXiv:1002.2419: https://arxiv.org/abs/1002.2419

## Verdict

Sound with minor caveats. The card correctly captures the doubling-over-precision trick. It should clarify that the logarithmic overhead applies to setup because setup is repeated at each level.

## Line-Anchored Comments

- Lines 10-18: algorithm is accurately summarized.

  The constants `14` and `50` are source-specific. They are fine to keep, but label them as proof constants rather than algorithm-design principles.

- Line 18: cost expression is good.

  The important nuance is that the update/checking work is dominated by the final precision level, while setup may be paid at each precision level, giving the `O(log T) S` term.

- Line 23: "any binary search over precision"

  This generalization is okay, but the trick requires a monotone-enough success threshold: once precision is sufficient, a constant number of repetitions must give constant success. Not every phase-estimation use has that structure.

- Line 29: caveat about setup bottleneck is excellent.

  Keep it; this is often the practical cost hidden by asymptotic walk-step counts.

## Missing or Suggested Additions

- Add that this does not remove the need for an estimate/lower bound on marked stationary probability in all KMO variants; it addresses unknown hitting-time/precision.
