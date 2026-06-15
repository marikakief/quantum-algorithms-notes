# Review: Szegedy Walk for Formula Evaluation

Source note: [[Szegedy Walk for Formula Evaluation]]

## Primary Sources Checked

- Childs, Reichardt, Spalek, Zhang, arXiv:quant-ph/0703015: https://arxiv.org/abs/quant-ph/0703015
- Ambainis, arXiv:0704.3628: https://arxiv.org/abs/0704.3628
- Szegedy, "Quantum speed-up of Markov chain based algorithms", checked as the spectral-theorem background.

## Verdict

Minor issues. The card is a good reusable abstraction, but it should be more careful about the exact Szegedy factorization conditions and phase conventions.

## Line-Anchored Comments

- Line 12: "disconnecting leaves that evaluate to 1"

  Correct for the NAND-tree encoding used in the source. It would help to mention that the formula value convention is tied to NAND; for AND/OR presentations, negations and tree transformations can change which leaves are removed.

- Line 14: `H = P circ P^T` condition is too compressed.

  The factorization depends on nonnegative weighted adjacency structure and normalization. It is not true for an arbitrary Hermitian formula Hamiltonian. State this as a constructed stochastic normalization, not a generic identity.

- Line 14: eigenphases `± arcsin lambda`

  Phase convention varies across Szegedy-style walks. In this formula-evaluation walk, zero-energy eigenstates are detected around phases `pi/2` and `3pi/2`. Make that the robust statement.

- Line 17: weight assignment explanation is good.

  Add that `s_v` counts leaves under `v` in the formula tree, so the weights depend only on known formula structure, not the input bits.

- Lines 25-27: complexity statements are correct.

  Later span-program algorithms improve the general-formula endpoint to exact `O(sqrt N)` in appropriate models; if this card is historical, add that context.

## Missing or Suggested Additions

- Add a short warning that "formula size" counts leaves with multiplicity; variables can appear more than once in the formula.
