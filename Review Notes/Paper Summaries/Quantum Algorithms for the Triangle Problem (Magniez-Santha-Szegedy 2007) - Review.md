# Review: Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007)

Source note: [[Quantum Algorithms for the Triangle Problem (Magniez-Santha-Szegedy 2007) — Paper Notes]]

## Primary Sources Checked

- Magniez, Santha, and Szegedy, "Quantum Algorithms for the Triangle Problem", arXiv: https://arxiv.org/abs/quant-ph/0310134
- Le Gall, "Improved Quantum Algorithm for Triangle Finding via Combinatorial Arguments", arXiv: https://arxiv.org/abs/1407.0085

## Verdict

Minor issues. The summary is strong and source-aligned. The main corrections are around lower-bound phrasing and a couple of subroutine labels.

## Line-Anchored Comments

- Lines 19-28: two algorithms and headline costs

  Correct. The source abstract gives `tilde O(n^{10/7})` for the combinatorial algorithm and `tilde O(n^{13/10})` for the quantum-walk algorithm, with `O(log n)` quantum space for the first and `O(n)` qubits for the second.

- Line 28 and lines 108-117: current best

  As of this check, `tilde O(n^{5/4})` from Le Gall remains the standard dense-graph query upper bound, with later work removing some logarithmic factors in related formulations. Keep "current best" dated if the vault is meant to stay stable.

- Lines 38-46: combinatorial cleaning

  Good. The explanation of cleaning high-common-neighborhood edges is clear.

- Line 44: amplitude estimation link

  This should probably be amplitude amplification/Grover search, not amplitude estimation via phase estimation. The paper's algorithmic ingredients here are Grover-style search and combinatorial filtering.

- Lines 58-70: generic collision framework

  Good. This is a clean way to connect the paper to Ambainis's Johnson-graph walk.

- Lines 72-89: recursive checking for triangle

  Good and source-consistent. The cost formula optimizes correctly at `r = n^{3/5}`.

- Lines 104 and 123-126: lower-bound/adversary wording

  Be careful. It is true that the known lower bound is only `Omega(n)`, but saying "the adversary method (all variants) cannot prove anything better" is not a stable theorem-level statement in the era of negative-weight adversary methods. Say the known techniques/results at the time only gave `Omega(n)`.

## Missing or Suggested Additions

- Add the model explicitly: adjacency-matrix query model. Sparse graph/edge-list models have different algorithms and should not be conflated with this note.
