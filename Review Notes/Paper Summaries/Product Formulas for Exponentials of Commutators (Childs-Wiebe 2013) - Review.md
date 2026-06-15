# Review: Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013)

Source note: [[Product Formulas for Exponentials of Commutators (Childs-Wiebe 2013) — Paper Notes]]

## Primary Sources Checked

- Childs and Wiebe, "Product Formulas for Exponentials of Commutators," arXiv:1211.4945: https://arxiv.org/abs/1211.4945

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 9-15: the computational problem is correctly separated from ordinary Hamiltonian simulation. This distinction should be preserved.
- Line 15: the lower bound is about the scaling for commutator simulation in this access model. Do not compare it directly to LCU/QSP precision scaling for ordinary `e^{-iHt}` simulation.
- Lines 25-39: the odd-`k` recursion is described clearly.
- Lines 73-79: the segmented-simulation scaling should keep the meaning of `T` and `t` explicit. The notation is easy to misread because the simulated commutator time is `T^{k+1}` in earlier sections.
- Lines 111-115: good caveat that constants are large and the commutator-simulation setting is distinct.

## Missing or Suggested Additions

- Add a one-sentence explanation of anti-Hermitian versus Hermitian conventions so signs and unitarity are clear.
