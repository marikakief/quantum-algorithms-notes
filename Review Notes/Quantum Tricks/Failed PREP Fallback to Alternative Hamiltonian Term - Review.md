# Review: Failed PREP Fallback to Alternative Hamiltonian Term

Source note: [[Failed PREP Fallback to Alternative Hamiltonian Term]]

## Primary Sources Checked

- Su et al., arXiv:2105.12767, qubitization construction and normalization formulas.

## Verdict

Minor issues. The card explains the cleverness well, but it treats the `p approx 1/4` intuition as if it were the exact theorem.

## Line-Anchored Comments

- Lines 7 and 11-18: the simplified `H1 + H2` demonstration is useful. Label it as the idealized case.
- Lines 19-23: the interpolation cases are where the real paper lives. Line 23 has a LaTeX typo: `(lambda_U + lambda_V)` is missing the backslash/formatting for `\lambda`.
- Line 23: the effective normalization in the source involves success probabilities and correction factors such as `P_eq`; the card should not imply exact optimal normalization in all regimes.
- Line 25: "success probability `approx 1/4`" is right as intuition for the nested-box preparation, but the paper accounts for deviations. Mention that finite precision and equality-test success modify the exact lambda.
- Line 38: the "saves a factor of 3" line is true relative to a single round of amplitude amplification for the expensive PREP branch, but the total block-encoding savings are smaller if other costs dominate.
- Line 42: good caveat. The trick only helps because SEL for kinetic energy is cheap in the source application.

## Missing or Suggested Additions

- Add a small formula for the non-ideal case or point directly to the paper's theorem expression; this is a normalization trick, so exact coefficients matter.

