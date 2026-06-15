# Review: LCU Origins (Childs-Wiebe 2012)

Source note: [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]

## Primary Sources Checked

- Childs and Wiebe, "Hamiltonian Simulation Using Linear Combinations of Unitary Operations", Quantum Information and Computation 2012 / arXiv:1202.5822.

## Verdict

Minor issues. The note is historically well positioned and correctly treats this as an origin paper rather than the modern PREPARE/SELECT formulation. Most issues are wording and attribution precision.

## Line-Anchored Comments

- Lines 9-12: good framing. "Recognizable form" is the right level of claim; earlier linear-combination ideas existed, but this paper made the simulation primitive explicit.
- Lines 15-21: the two-unitary addition gadget is described clearly. The success/failure expression should define whether `Delta` is an operator norm and whether the displayed bound is failure probability or failure amplitude.
- Lines 21 and 39: typo/style: `multi-[[Product Formulas]]s` should be "multi-product formulas" or linked without the extra plural.
- Lines 27-31: the complexity statement should say which norm convention and Hamiltonian decomposition model it uses. `m`, `h`, and `t` are enough for an expert, but the note later compares to sparse-oracle algorithms with different parameters.
- Line 31: "much better than product formulas" is a little imprecise because the algorithm is built from multi-product formulas. Better: "better than single product-formula sequences in precision scaling, but not yet Taylor/QSP polylog precision."
- Lines 55-64: the modern-perspective section is good. PREPARE/SELECT did not appear in this final form here, and that distinction is important.
- Lines 82-87: this section is titled "References within this paper" but includes later papers. Split actual in-paper references from later vault cross-links.

## Missing or Suggested Additions

- Add one sentence explaining why negative coefficients are harder than positive LCU coefficients: they force subtraction/failure handling rather than a straightforward postselected weighted sum.
