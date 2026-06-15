# Review: Optimal-Order Product Formula Selection

Source note: [[Optimal-Order Product Formula Selection]]

## Primary Sources Checked

- Berry et al. sparse-Suzuki analysis, arXiv:quant-ph/0508139: https://arxiv.org/abs/quant-ph/0508139
- Morales et al. optimized product formula selection, arXiv:2210.15817: https://arxiv.org/abs/2210.15817

## Verdict

Needs rewrite. The core idea is right, but the formula for the optimal order is wrong and contradicts the card's own final asymptotic expression.

## Line-Anchored Comments

- Lines 9-13: the tradeoff between fewer steps and larger formula length is correct.
- Lines 15-19: the displayed `k* approx (1/2) ln(Lambda t/epsilon)/ln 5` is wrong for the standard Suzuki optimization. The usual optimized-order estimate scales like a square root of a logarithm, consistent with the subexponential `exp(O(sqrt(log(...))))` bound in line 35.
- Line 19: the FeMo `k* approx 6` claim may be from a concrete resource estimate with additional constants, but it should not be derived from the incorrect formula in line 17.
- Lines 21-23: the practical rule of thumb may be useful, but it needs a cited source and cost-model assumptions.
- Line 35: the final `exp(O(sqrt(log(...))))` expression is the right qualitative asymptotic for optimized Suzuki order; use that to repair the derivation.
- Line 37: Taylor/QSP comparison again needs normalization and the additive precision distinction.

## Missing or Suggested Additions

- Replace the derivative calculation with the standard BACS/Suzuki optimized-order calculation, then separately discuss empirical/resource-estimate choices such as 6th or 8th order.
