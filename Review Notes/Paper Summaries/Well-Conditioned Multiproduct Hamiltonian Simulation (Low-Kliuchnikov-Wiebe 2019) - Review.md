# Review: Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019)

Source note: [[Well-Conditioned Multiproduct Hamiltonian Simulation (Low-Kliuchnikov-Wiebe 2019) — Paper Notes]]

## Primary Sources Checked

- Low, Kliuchnikov, Wiebe, "Well-conditioned multiproduct Hamiltonian simulation," arXiv:1907.11679: https://arxiv.org/abs/1907.11679

## Verdict

Major issues. The main Chebyshev-conditioning story is right, but the LCU coefficient handling and comparison claims need tightening.

## Line-Anchored Comments

- Line 17: source abstract says the method has worst-case `O(t log^2(t/epsilon))` scaling up to model normalization. The note's `lambda` insertion is fine only if `lambda` is explicitly defined for the base formula model.
- Line 37: `|a> = sum sqrt(a_j)|j>` is wrong for signed coefficients. Use `sqrt(|a_j|/||a||_1)` and put signs/phases into SELECT. Line 79 later does this correctly.
- Lines 78-82: the LCU implementation description is good after the coefficient fix.
- Line 112: QSP comparison should not use raw `O(t + log(1/epsilon))` without normalization and precision-term caveats.
- Line 116: "the only method" is too strong. Say it is a distinctive method combining commutator sensitivity with near-optimal time/error scaling, not uniquely so in every model.
- Line 159: "This is Marika's paper" is informal metadata. It may be useful locally, but it does not belong in a scholarly paper-summary note unless the vault intentionally keeps personal relevance notes.

## Missing or Suggested Additions

- Add a one-line definition of the condition number/resolution factor in both coherent LCU and randomized-MPF contexts.
