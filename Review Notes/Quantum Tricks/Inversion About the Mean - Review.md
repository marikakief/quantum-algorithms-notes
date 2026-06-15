# Review: Inversion About the Mean

Source note: [[Inversion About the Mean]]

## Primary Sources Checked

- Grover arXiv: https://arxiv.org/abs/quant-ph/9605043
- BBHT tight-search analysis: https://arxiv.org/abs/quant-ph/9605034

## Verdict

Minor issues. The core formula and explanation are correct.

## Line-Anchored Comments

- Lines 11-25: definition and amplitude update

  Correct for the uniform reference state. Good derivation.

- Line 29: "Cost: O(n) gates"

  Add assumptions. The multi-controlled phase about `|0^n>` is not a primitive one-gate operation in most gate sets. It can be implemented with `O(n)` Toffoli-scale gates with ancilla choices, or different costs depending on fault-tolerant gate set and clean/dirty ancilla availability.

- Lines 33-36: relationship to amplitude amplification and fixed-point variants

  Correct, but these are later developments. Label them as descendants rather than Grover-paper content.

- Line 44: overshoot caveat

  Good. For generalized search, the iteration count depends on the marked fraction, not just `N`.

## Missing or Suggested Additions

- Add a sign-convention note: some sources use `I - 2|s><s|`; global signs do not change measurement probabilities but affect product-of-reflections formulas.
