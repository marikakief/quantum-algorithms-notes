# Review: Hamiltonian Simulation by Qubitization (Low-Chuang 2019)

Source note: [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]

## Primary Sources Checked

- Low and Chuang, "Hamiltonian Simulation by Qubitization", Quantum 2019 / arXiv:1610.06546.

## Verdict

Major issues. The geometric explanation is good, but the main complexity statement is too loose and is presented as the optimal benchmark. For this paper, the normalization and precision term need to be exact.

## Line-Anchored Comments

- Add a top-level title; the note begins with a blank line and metadata.
- Lines 10-20: good explanation of why qubitization clarifies the older LCU/walk machinery.
- Lines 24-30: standard-form encoding is stated well. Add the normalization parameter explicitly; the encoded operator is usually `H/alpha`, and the simulation cost scales with `alpha t`.
- Lines 32-36: the invariant two-dimensional subspace description is the core of the paper and is well written.
- Lines 44-48: `O(t + log(1/epsilon))` is not the right optimal benchmark. In normalized units the sharp query scaling is of the form `O(alpha t + log(1/epsilon)/loglog(1/epsilon))` up to conventions. A looser `O(alpha t + log(1/epsilon))` upper bound is not what should be called optimal.
- Line 60: "gives a quadratic speedup for precision simulations" is unclear. Specify which earlier method and which parameter are being quadratically improved.
- Lines 62-70: some listed references are later than the original arXiv date. Split "references within this paper" from "later cross-links."
- Lines 85-94: useful application links, but the source note should not rely on them to supply missing theorem details.

## Missing or Suggested Additions

- Include the exact qubitization iterate definition and the relation between eigenvalue `lambda/alpha` and the walk eigenphases.
- Add the QSP polynomial approximation degree for `e^{-i alpha t x}` so the complexity statement is checkable.
