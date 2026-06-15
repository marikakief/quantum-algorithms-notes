# Review: Suzuki Order as a Tunable Knob for Time Scaling

Source note: [[Suzuki Order as a Tunable Knob for Time Scaling]]

## Primary Sources Checked

- Berry, Ahokas, Cleve, Sanders, arXiv:quant-ph/0508139: https://arxiv.org/abs/quant-ph/0508139
- Childs et al., "A Theory of Trotter Error," arXiv:1912.08854: https://arxiv.org/abs/1912.08854

## Verdict

Sound with minor caveats. The optimization intuition is right, but the comparison to later methods and "best product formulas can do" language should be narrowed.

## Line-Anchored Comments

- Lines 11-27: the formula captures the BACS optimization story. Make clear this is the Suzuki-recursion bound, not a universal lower bound on all product-formula variants.
- Line 29: the QSP comparison needs normalization and the usual additive precision term; avoid raw `O(||H||t + log(1/epsilon))`.
- Line 34: "best product formulas can do asymptotically" is too broad in a modern vault that also contains randomized, commutator-aware, and multiproduct formulas. Say "best within this Suzuki-recursion analysis."
- Lines 37-39: the practical caveat is good and should stay.

## Missing or Suggested Additions

- Cross-link explicitly to modern commutator-scaling bounds so the card is not read as the final word on product-formula performance.
