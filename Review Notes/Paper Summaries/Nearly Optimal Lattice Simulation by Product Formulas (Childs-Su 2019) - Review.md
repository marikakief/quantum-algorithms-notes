# Review: Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019)

Source note: [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]]

## Primary Sources Checked

- Childs and Su, "Nearly optimal lattice simulation by product formulas," arXiv:1901.00564: https://arxiv.org/abs/1901.00564

## Verdict

Sound with minor caveats. This is a strong summary of the local-error representation and why product formulas remain theoretically important.

## Line-Anchored Comments

- Lines 9-12: the headline `(nt)^{1+o(1)}` and linear-in-`n` local error message are correct.
- Lines 47-55: the first-order local-commutator derivation is clear and pedagogically useful.
- Lines 79-85: the gate-complexity table should state whether `epsilon` is constant in the final `(nt)^{1+delta}` line. The dependence is present for fixed order.
- Lines 93-97: the comparison table should avoid implying that commutator bounds from the sister paper are always superlinear in `n`; the exact scaling depends on how the commutator sum is bounded.
- Line 137: "Product formulas win practically" should be attributed to the paper's tested regimes/figure, not stated as a universal rule.

## Missing or Suggested Additions

- Add a small note that the result is for geometrically local lattice Hamiltonians; sparse black-box Hamiltonians are a different model.
