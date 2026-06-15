# Review: Eigenvalue Error as the Correct Product Formula Metric

Source note: [[Eigenvalue Error as the Correct Product Formula Metric]]

## Primary Sources Checked

- Morales et al., arXiv:2210.15817: https://arxiv.org/abs/2210.15817

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 7-17: the eigenvalue-versus-basis-error distinction is well explained.
- Line 29: "phase estimation" is a good application, but say this assumes repeated use of the same product-formula step so basis error does not accumulate in the same way.
- Line 30: the warning about randomized formulas is useful, but should be stated as a limitation/hypothesis rather than a blanket conclusion about all randomization.
- Lines 37-39: the caveat should say spectral norm remains the safe worst-case metric; eigenvalue error is a sharper selection metric for repeated-step long-time simulation, not a universal replacement.

## Missing or Suggested Additions

- Add the conditions under which basis error remains bounded: same approximate step repeated and diagonalizability/nearby eigenbasis assumptions.
