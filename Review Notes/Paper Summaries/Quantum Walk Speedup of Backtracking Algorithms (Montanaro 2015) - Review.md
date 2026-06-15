# Review: Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015)

Source note: [[Quantum Walk Speedup of Backtracking Algorithms (Montanaro 2015) — Paper Notes]]

## Primary Sources Checked

- Montanaro, "Quantum walk speedup of backtracking algorithms," Theory of Computing 14(15), 1-24 (2018), arXiv:1509.02374.

## Verdict

Minor issues. This is a clear and mostly accurate note. The main technical fixes are notation around vertex degree, the role of the upper bound on tree size, and the limited meaning of the average-case exponential separation.

## Line-Anchored Comments

- Lines 9-17: good definition of the backtracking model. If the domain size `d` is not constant, the branching-degree dependence should be included in implementation costs.
- Lines 23-25: excellent explanation of why this is better than Grover over all assignments when pruning is effective.
- Lines 35-40: the diffusion-state normalization is notation-sensitive. If `d_x` means number of children, the denominator for non-root vertices should be `sqrt(d_x+1)`; if it means degree including the parent, say so explicitly.
- Lines 47-62: the detection algorithm is well explained. Keep the phase-estimation precision tied to an upper bound on `T`; this is not a parameter-free procedure.
- Lines 66-73: the binary-search search procedure is fine for constant branching. For nonconstant domains, "check each child subtree" adds degree dependence.
- Lines 83-85: good theorem statement. It would be useful to separate detection, general search, and unique-solution search into three lines because the overheads differ.
- Lines 87 and 99-100: "O(1) auxiliary operations per evaluation" is too terse. Local diffusion around a vertex must compute the children determined by `P` and `h`; the query model hides most of this in evaluations.
- Lines 108-116: the average-case exponential speedup needs an explicit warning. It is for a constructed/heavy-tailed distribution over instances and a fixed deterministic backtracking heuristic, not a theorem saying random 3-SAT is generically exponentially easier quantumly.
- Lines 122-130: strong caveats. The "lucky classical search" issue is especially important because real SAT solvers care about time-to-first-solution, not just full-tree size.

## Missing or Suggested Additions

- Add a small box defining `T`, depth `n`, branching degree, marked vertices, and whether `T` is known or obtained by doubling.

