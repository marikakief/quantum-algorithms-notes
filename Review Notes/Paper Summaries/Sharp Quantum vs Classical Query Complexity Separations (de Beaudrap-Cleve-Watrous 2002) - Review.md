# Review: Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002)

Source note: [[Sharp Quantum vs Classical Query Complexity Separations (de Beaudrap-Cleve-Watrous 2002) — Paper Notes]]

## Primary Sources Checked

- de Beaudrap, Cleve, and Watrous, "Sharp quantum versus classical query complexity separations", Algorithmica 2002.

## Verdict

Minor issues. The field-structured query separation is summarized well. The comparison table should not put Shor's factoring/order-finding result on the same footing without clarifying the oracle model.

## Line-Anchored Comments

- Add a top-level title.
- Lines 15-23: good description of the exact one-query quantum algorithm and the classical birthday-style lower bound.
- Lines 29-54: the finite-field QFT/control-target inversion explanation is useful and accurate at the right level.
- Lines 64-66 and 103: excellent caveat about field versus ring structure. This is one of the easiest mistakes to make with this paper.
- Lines 73-75: the query count and auxiliary-operation distinction is clear.
- Lines 88-93: the comparison table needs care. The Shor row's "O(1)" quantum query entry is not directly comparable to this paper's oracle promise problem unless the row is explicitly about a period/order-finding black-box abstraction. Factoring as an input-length problem is not an `O(1)`-query problem in the same sense.
- Line 93: the Forrelation comparison is okay if the note keeps saying "partial/promise function" and "bounded-error."

## Missing or Suggested Additions

- Add one sentence defining the exact problem oracle before the QFT construction. The current note explains the mechanism but the problem statement is relatively implicit.

