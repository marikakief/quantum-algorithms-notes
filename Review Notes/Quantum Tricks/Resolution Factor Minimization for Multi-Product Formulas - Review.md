# Review: Resolution Factor Minimization for Multi-Product Formulas

Source note: [[Resolution Factor Minimization for Multi-Product Formulas]]

## Primary Sources Checked

- Faehrmann et al., arXiv:2101.07808: https://arxiv.org/abs/2101.07808
- Low, Kliuchnikov, Wiebe, arXiv:1907.11679: https://arxiv.org/abs/1907.11679

## Verdict

Sound with minor caveats, assuming the companion randomized-sampling card is corrected.

## Line-Anchored Comments

- Lines 9-13: the role of `Xi = sum |C_q|` is correct.
- Lines 19-27: matching MPF is explained clearly. Add the regime assumptions: the bound is useful when the segment norm is small enough that the bracket is close to 1.
- Lines 33-36: the closed-form/Gauss-Legendre discussion should be checked carefully. If the nodes are not integer step counts, the implementation requires rounding or a different interpretation; the card notes this but should not say `Xi = 1... almost` too casually.
- Line 54: the Watson/qFLO comparison is plausible later context, but "doing the same thing as matching MPF" is too strong. Say it is an analogous effort to keep Richardson/extrapolation weights well conditioned.

## Missing or Suggested Additions

- Add the connection between `Xi` and the two-index observable estimator, not just the informal "signed linear combination" explanation.
