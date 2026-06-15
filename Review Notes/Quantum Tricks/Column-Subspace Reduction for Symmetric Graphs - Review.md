# Review: Column-Subspace Reduction for Symmetric Graphs

Source note: [[Column-Subspace Reduction for Symmetric Graphs]]

## Primary Sources Checked

- Childs et al., "Exponential algorithmic speedup by quantum walk", arXiv:quant-ph/0209131: https://arxiv.org/abs/quant-ph/0209131
- Childs, Farhi, Gutmann, arXiv:quant-ph/0103020, for the simpler glued-tree reduction: https://arxiv.org/abs/quant-ph/0103020

## Verdict

Minor issues. This card is mostly correct and complements the older CFG-specific symmetry card. It just needs a more precise formula for effective couplings and a weaker automorphism assumption.

## Line-Anchored Comments

- Lines 10-14: invariant-subspace condition is right in spirit.

  The necessary condition is biregularity between the partition cells: every vertex in a column should see the same number of neighbors in each adjacent column, with compatible counts in the reverse direction. Full permutation symmetry within each column is sufficient but not necessary.

- Line 16: effective matrix element formula is incomplete.

  In general, the coupling is proportional to the number of edges between the two columns divided by `sqrt(N_j N_{j+1})`, equivalently `gamma d_{j->j+1} sqrt(N_j/N_{j+1})` in a biregular partition. This reduces to `gamma sqrt(d)` for a regular branching step, but the card's formula hides the dependence on column sizes.

- Line 18: random inter-column connections caveat is good.

  This is exactly why the 2003 random cycle can preserve the column reduction while defeating classical orientation/backtracking.

- Line 23: "single orbit of the automorphism group"

  Too strong. A single orbit is a clean sufficient condition, but equitable/biregular partitions can exist without a full automorphism orbit.

## Missing or Suggested Additions

- Add the term "equitable partition"; it is the standard graph-theoretic vocabulary for this reduction.
