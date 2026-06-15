# Review: Fixed-Angle Randomized Product Formula

Source note: [[Fixed-Angle Randomized Product Formula]]

## Primary Sources Checked

- Campbell, arXiv:1811.08017: https://arxiv.org/abs/1811.08017

## Verdict

Sound but underdeveloped.

## Line-Anchored Comments

- Lines 5-9: the fixed-angle point is correct: coefficient magnitudes move into sampling probabilities, leaving a common rotation angle.
- Line 9: "deterministic product formulas where each term may need a different angle" should be qualified. Deterministic formulas often use term-dependent coefficients `h_j delta t`; qDRIFT's common angle is with respect to normalized Pauli/unit terms.
- Lines 14-18: the complexity section is too terse. Add the actual qDRIFT error scaling `N = O((lambda t)^2/epsilon)`.

## Missing or Suggested Additions

- Include a short warning that fixed-angle sampling is a channel simulation method, not a deterministic unitary formula.
