# Review: Block-Encoding Composition Algebra

Source note: [[Block-Encoding Composition Algebra]]

## Primary Sources Checked

- Gilyen, Su, Low, Wiebe, arXiv:1806.01838: https://arxiv.org/abs/1806.01838

## Verdict

Sound with minor caveats. The card is intentionally high-level and gets the main warning right: normalization is the enemy.

## Line-Anchored Comments

- Lines 8-12: good summary of why block-encodings modularize matrix algorithms.
- Lines 16-19: the rules of thumb are useful, but they should mention error accumulation and ancilla/projector compatibility. Products and sums are not purely algebraic; their block locations must line up.
- Line 20: normalization factors may multiply, add, or be rescaled depending on the construction. The current statement is fine as a warning but not as a formula.
- Line 21: this is the strongest sentence in the card and should stay.

## Missing or Suggested Additions

- Add one concrete rule: product of an `alpha` block-encoding of `A` and a `beta` block-encoding of `B` gives an `alpha beta` block-encoding of `AB`, up to errors and compatible ancillas.
