# Review: Chebyshev Nesting (Semigroup Concatenation)

Source note: [[Chebyshev Nesting (Semigroup Concatenation)]]

## Primary Sources Checked

- Yoder-Low-Chuang fixed-point search: https://arxiv.org/abs/1409.3305

## Verdict

Minor issues. Good conceptual card, but the operational/adaptive language should be more precise.

## Line-Anchored Comments

- Lines 12-18: semigroup property

  Correct high-level mechanism: Chebyshev composition underlies nesting.

- Lines 20 and 30: "without restarting" / "check if result is good enough"

  These pull in different operational models. Coherent nesting can extend a sequence without restarting, but checking by measurement collapses the state and is not the same coherent prefix extension. Rephrase as either coherent sequence extension or measurement-based adaptive restart, not both at once.

- Lines 22-26: nested phase angles

  This is formula-sensitive and should include a reference to the exact equation/appendix in Yoder-Low-Chuang. A wrong index convention here would be hard for readers to catch.

- Line 37: "complexities L1, L2 uses l1 + 2l1l2 + l2 Grover iterates"

  Needs verification against the paper's definition of `L = 2l + 1` and what counts as an oracle query versus a generalized Grover iterate. If left, define the counting convention.

## Missing or Suggested Additions

- Add a simple worked example with small `L1, L2` or remove the explicit phase-index formula until fully checked.
