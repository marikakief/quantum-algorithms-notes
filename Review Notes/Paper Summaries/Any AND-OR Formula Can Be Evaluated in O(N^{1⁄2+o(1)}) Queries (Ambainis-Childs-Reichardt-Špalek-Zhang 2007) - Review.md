# Review: Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007)

Source note: [[Any AND-OR Formula Can Be Evaluated in O(N^{1⁄2+o(1)}) Queries (Ambainis-Childs-Reichardt-Špalek-Zhang 2007) — Paper Notes]]

## Primary Sources Checked

- Ambainis, Childs, Reichardt, Špalek, and Zhang, "Every AND-OR formula of size N can be evaluated in time N^{1/2+o(1)} on a quantum computer," FOCS 2007.
- Childs, Reichardt, Špalek, and Zhang, arXiv:quant-ph/0703015; Ambainis, arXiv:0704.3628.

## Verdict

Minor issues. This is a strong summary of a technically subtle pair of papers. The main risks are overcompressing the walk/Hamiltonian conversion and underemphasizing that the full `N^{1/2+o(1)}` result uses formula rebalancing/preprocessing.

## Line-Anchored Comments

- Lines 11-13: good problem framing. The balanced-tree classical `N^{0.754...}` statement is for a specific randomized alpha-beta pruning setting; keep it tied to balanced game trees.
- Lines 17-21: the theorem statement and Laplante-Lee-Szegedy consequence are good.
- Lines 29-40: the weighted adjacency construction is useful. The special tail weight is not a detail: it is part of making the zero-energy overlap test work, so do not omit it in a shorter version.
- Lines 45-48: good spectral dichotomy. Define `sigma_-(r)` and `sigma_+(r)` before quoting the gap bound elsewhere.
- Lines 52-57: the Szegedy/coined-walk conversion is plausible but terse. Check the exact normalization of `H` versus the stochastic matrix before turning this into a reusable trick card.
- Lines 61-64: phase-estimation output around `0` or `T/2` depends on the phase convention and the `(-i)^t` shift in the balanced algorithm. Keep that convention visible.
- Lines 67-68: the rebalancing parameter calculation is important and should stay. This is what turns balanced-tree evaluation into arbitrary-formula evaluation.
- Lines 75-79: the balanced-case circuit sketch is good and concrete.
- Lines 91-94: the result table is useful. Reichardt's later span-program `O(sqrt(N))` result for read-once formulas should be mentioned in the table or caveat, not only at line 114.
- Lines 114-116: caveats are good. "Only evaluates, does not find a winning strategy" is an important distinction.

## Missing or Suggested Additions

- Add a glossary for `s_v`, `sigma_+`, `sigma_-`, tail vertices, and the phase convention used by the walk.

