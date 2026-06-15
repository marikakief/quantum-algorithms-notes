# Review: Gramian-Based Momentum Norm for Non-Cubic Cells

Source note: [[Gramian-Based Momentum Norm for Non-Cubic Cells]]

## Primary Sources Checked

- Berry et al., arXiv:2312.07654.

## Verdict

Minor issues. The card is technically solid. Most edits are about not overgeneralizing the material-specific arithmetic simplifications.

## Line-Anchored Comments

- Lines 7 and 13-17: the Gramian quadratic-form setup is right and clearly explained.
- Lines 21-27: the crystal-class table is useful. Because these are structure-specific reductions, label them as examples from the paper rather than a general classification theorem.
- Line 26: the chemical formulas contain non-ASCII subscripts; that is fine for readability, but be consistent with the rest of the vault's style.
- Line 28: the doubled-cost statement for inner products should specify which part doubles: integer arithmetic from squarings to cross-products, not every real-constant multiplication.
- Line 30: absorbing one overall scale factor is a good subtlety and should stay.
- Line 44: the rough Toffoli examples depend on `n` and `b` conventions. Fine as intuition, but avoid using them as benchmark numbers without source table references.
- Line 50: the statement that controlled swaps dominate is system-size dependent. For low-symmetry small cells, arithmetic can be non-negligible.

## Missing or Suggested Additions

- Add a one-sentence bridge to why the older bit-product kinetic trick no longer applies cleanly in non-cubic lattices.

