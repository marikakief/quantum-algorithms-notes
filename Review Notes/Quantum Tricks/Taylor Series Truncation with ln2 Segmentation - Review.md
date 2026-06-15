# Review: Taylor Series Truncation with ln2 Segmentation

Source note: [[Taylor Series Truncation with ln2 Segmentation]]

## Primary Sources Checked

- BCCKS truncated Taylor, arXiv:1412.4687: https://arxiv.org/abs/1412.4687

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 9-12: because `r = ceil(T/ln 2)`, the segment parameter is generally `tau = T/r <= ln 2`, not exactly `ln 2` for every segment unless the algorithm pads or adjusts segments.
- Lines 12-14: the coefficient sum is exactly 2 only for the infinite series at `tau = ln 2`. With truncation and arbitrary final segment length, amplitude dilution/adjustment is part of the story.
- Line 16: the truncation-order scaling should be written with `T/epsilon`, as the card does, because the per-segment error target depends on the number of segments.
- Line 26: "near-optimal in all parameters" is too strong without saying this is near-optimal for the Taylor-LCU algorithm and not the later additive-precision QSP/qubitization optimum.
- Line 29: "SELECT handles this via K sequential controlled operations" is right at the query-structure level, but PREPARE over `K` term registers still has real implementation cost.

## Missing or Suggested Additions

- Add a small note that the `ln 2` choice is chosen to make single-round robust OAA convenient, not because `ln 2` is intrinsically required for Taylor truncation.
