# Review: Unary Iteration

Source note: [[Unary Iteration]]

## Primary Sources Checked

- Babbush et al., arXiv:1805.03662, Section III.A.

## Verdict

Minor issues. The construction is mostly accurate, but the headline should state that the `4L-4` cost excludes the cost of the controlled operations themselves.

## Line-Anchored Comments

- Line 7: `4L-4` is the overhead for generating/using the unary controls under the paper's temporary-AND accounting. It does not include non-Clifford cost inside arbitrary `U_l`.
- Lines 17-22: the description is a bit more tree-like than the actual streaming construction, but it conveys the reuse of comparisons/ANDs well enough for a trick card.
- Line 22: "uncomputing requires 0 additional T gates" relies on measurement-based uncomputation of temporary logical-AND ancillae. Say this explicitly and note the required measurement/feedforward model.
- Line 26: the XOR accumulator for Jordan-Wigner strings is a useful detail and should remain.
- Line 43: "the whole point of the plane-wave dual basis is to keep `L=O(N^2)`" is not quite aligned with the line's preceding `L=O(N)` example. Clarify whether `L` is the number of unary branches for a given subroutine or the total number of Hamiltonian terms.
- Line 45: good caveat. Many summaries forget that the 0-T uncompute is architecture/model-dependent.

## Missing or Suggested Additions

- Add one sentence distinguishing unary iteration from unary encoding of a time/Taylor-order register; the related-notes section hints at this but the main text does not.

