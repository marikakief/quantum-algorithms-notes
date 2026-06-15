# Review: Permutation-Observable Noise Deconvolution for Multi-Copy Moments

Source note: [[Permutation-Observable Noise Deconvolution for Multi-Copy Moments]]

## Primary Sources Checked

- Kannan, Putterman, and Cotler, arXiv:2605.02057, checked current arXiv v1.

## Verdict

Minor issues. The card is accurate at the conceptual level. It just needs stronger warnings around variance and noise-model calibration.

## Line-Anchored Comments

- Lines 12-24: the cubic-moment observable is clear.
- Lines 26-47: the inverse-noise-on-observable construction is well explained.
- Lines 49-55: the one-site eigenvalue estimator is useful, but define the local diagonalization basis or point to the source equation.
- Lines 65-76: the variance/sample-complexity formula is version-sensitive and should be checked against the source before reuse. The qualitative dependence on inverse powers of `1-lambda'` is the safer takeaway.
- Lines 78-82: excellent caveat. Put "requires calibrated Pauli-diagonal or otherwise invertible noise model" in the opening paragraph.

## Missing or Suggested Additions

- Add a short note that deconvolution is classical post-processing of measurement outcomes, not physical error correction.

