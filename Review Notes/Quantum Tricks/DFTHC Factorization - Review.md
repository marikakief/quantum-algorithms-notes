# Review: DFTHC Factorization

Source note: [[DFTHC Factorization]]

## Primary Sources Checked

- Low et al., arXiv:2502.15882 / Physical Review X 15, 041016 (2025).
- Lee et al., arXiv:2011.03494 / PRX Quantum 2, 030305 (2021), for THC background.

## Verdict

Minor issues. The factorization idea is described well, but the scaling prose should be tightened.

## Line-Anchored Comments

- Line 17: the DF-rank and THC-rank discussion should be stated as empirical/scaling-model behavior unless a theorem is cited for the instance class.
- Line 26: `N^2.09` is not "near-linear"; it is near-quadratic. If the intended comparison is "much better than older high-degree scalings", say that directly.
- Lines 31-36: the optimizer-cost warning is good and important. Keep the `1e5-1e6` step discussion near any claim about practical factorization quality.
- Lines 44-50: if the card compares DFTHC to THC or DF, specify whether the comparison is in `lambda`, Toffoli count, classical preprocessing, or memory.

## Missing or Suggested Additions

- Add a small parameter glossary for `R`, `B`, `C`, and THC rank. This card will otherwise be hard to use as a reusable reference.

