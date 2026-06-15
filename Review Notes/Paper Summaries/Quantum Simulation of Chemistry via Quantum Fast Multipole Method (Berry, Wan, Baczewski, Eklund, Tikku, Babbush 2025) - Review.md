# Review: Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025)

Source note: [[Quantum Simulation of Chemistry via Quantum Fast Multipole Method (Berry, Wan, Baczewski, Eklund, Tikku, Babbush 2025) — Paper Notes]]

## Primary Sources Checked

- Berry et al., "Quantum simulation of electronic structure via quantum fast multipole method", arXiv:2510.07380. Checked arXiv v2, revised 2026-05-03.

## Verdict

Major issues. The note is useful, but it appears to reflect an older version or an internal inconsistency in the crossover exponent, and several subroutine costs use incompatible log factors.

## Line-Anchored Comments

- Lines 21-25: the high-level real-space first-quantized/product-formula story is good.
- Line 25: version-sensitive correction. Current arXiv v2 says the approach has lower complexity for `N < eta^7`, not `N < eta^6`. If the note intentionally summarizes v1, record that explicitly; otherwise update all `eta^6` crossover statements.
- Line 29: the `eta >= 10^3` caveat is in the current abstract and is an important strength of the note.
- Line 37: linking this to the plane-wave first-quantized encoding may mislead readers; the paper uses a real-space grid representation. The relation is first-quantized electronic structure, not specifically plane waves.
- Lines 69 and 95: the sorting/routing costs disagree (`O(eta log^2 eta log N)` versus `O(eta log eta log^2 N)`). Pick the expression used by the paper for the named subroutine and do not interchange `log eta` and `log N`.
- Lines 75 and 97: near-field and potential-evaluation dominance are described with different costs. Clarify which one is per level, per product-formula segment, or after adaptive precision summation.
- Line 130: update the interaction-picture crossover to match the current `eta^7` version if summarizing v2.
- Line 146: the "no concrete resource estimates" caveat is excellent and should remain prominent.

## Missing or Suggested Additions

- Add a version note near the source citation. Since v2 appeared on 2026-05-03, this paper is moving enough that numerical crossovers should be dated.

