# Review: Recursive Quantum Walk for Nested Collision Problems

Source note: [[Recursive Quantum Walk for Nested Collision Problems]]

## Primary Sources Checked

- Magniez, Santha, and Szegedy, "Quantum Algorithms for the Triangle Problem", arXiv: https://arxiv.org/abs/quant-ph/0310134

## Verdict

Minor issues. The card accurately captures the triangle-finding recursive-walk pattern. It should make the query model and "known graph" assumption explicit.

## Line-Anchored Comments

- Lines 12-17: generic costs

  Good. The `s(r), u(r), c(r)` decomposition is the right way to explain the recursive checking cost.

- Lines 19-24: checking by graph collision

  Correct for the triangle algorithm: the outer subset's induced graph is stored/known, and for each external vertex one solves a graph-collision instance using oracle access to adjacency-to-`v` bits.

- Lines 25-28: cost formula and optimization

  Correct. With `s(r)=r^2`, `u(r)=r`, and `c(r)=tilde O(sqrt(n) r^{2/3})`, the optimum `r=n^{3/5}` gives `tilde O(n^{13/10})`.

- Lines 36-40: subgraph finding

  Good as source-era context. Add "for the MSS framework" or "in this paper" because later learning-graph/subgraph algorithms improved several constants/exponents.

- Lines 42-46: caveat

  Good. The known-subgraph assumption is crucial.

## Missing or Suggested Additions

- State explicitly that the paper works in the adjacency-matrix query model for the input graph.
