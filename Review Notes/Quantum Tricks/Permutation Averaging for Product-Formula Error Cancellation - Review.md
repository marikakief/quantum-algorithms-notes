# Review: Permutation Averaging for Product-Formula Error Cancellation

Source note: [[Permutation Averaging for Product-Formula Error Cancellation]]

## Primary Sources Checked

- Childs, Ostrander, Su, arXiv:1805.08385: https://arxiv.org/abs/1805.08385

## Verdict

Major issue in the stated cancellation mechanism; otherwise the card is useful.

## Line-Anchored Comments

- Lines 8-14: the mechanism is not that nondegenerate Taylor terms "average to zero." The averaged nondegenerate terms match the exact exponential's corresponding nondegenerate terms, so the error contribution cancels when comparing to the target.
- Line 14: degenerate terms survive because they are not fully symmetrized by distinct-index averaging; this part is right.
- Lines 16-24: the applicability and limitation statements are good.

## Missing or Suggested Additions

- Replace "average to zero" with "average to the target exponential's nondegenerate contribution." That is the core theorem-level distinction.
