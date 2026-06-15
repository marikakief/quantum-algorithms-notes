# Review: Continuous-Time Quantum Walk Search

Source note: [[Continuous-Time Quantum Walk Search]]

## Primary Sources Checked

- Childs and Goldstone, "Spatial search by quantum walk", arXiv:quant-ph/0306054: https://arxiv.org/abs/quant-ph/0306054
- Farhi and Gutmann analog Grover search checked as background.

## Verdict

Minor issues. The trick card captures the core mechanism, but it inherits the source note's sign-convention and low-dimensional cost ambiguity.

## Line-Anchored Comments

- Line 7: Hamiltonian convention needs a parenthetical.

  `H = -gamma L + |w><w|` is fine if `L=A-D`; it is the negative of the more standard `gamma(D-A)-|w><w|` convention. Say explicitly that an overall sign does not change measurement probabilities but reverses ground/highest-energy language.

- Lines 14-15: "find with O(1) probability"

  Correct only in the high-dimensional cases, e.g. complete graph, hypercube, and periodic lattices with `d>4`. In `d=4` the raw success probability is logarithmically small; below 4 it vanishes polynomially.

- Lines 21-25: table should separate raw evolution and amplified/repeated search cost.

  The table currently invites the reader to treat the listed time as the cost of finding the marked vertex. For `d=4` and below, that is not right without accounting for success probability.

- Line 30: "expander" example is not supported by this card.

  Childs-Goldstone analyze periodic lattices, the complete graph, and the hypercube. A general expander may have good spectral properties, but this specific avoided-crossing analysis does not automatically apply.

- Lines 40-42: caveat is good.

  Add that the coined/discrete-time low-dimensional lattice results use different walk dynamics, so they should not be presented as merely tuning the same Hamiltonian.

## Missing or Suggested Additions

- Add a "model" line: continuous-time oracle term plus graph-local hopping, not standard discrete query access unless separately simulated.
