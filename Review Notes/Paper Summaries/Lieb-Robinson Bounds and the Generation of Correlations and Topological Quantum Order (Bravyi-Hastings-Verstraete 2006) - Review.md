# Review: Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006)

Source note: [[Lieb-Robinson Bounds and the Generation of Correlations and Topological Quantum Order (Bravyi-Hastings-Verstraete 2006) — Paper Notes]]

## Verdict

Minor issues. The summary is mathematically solid and explains the four consequences clearly. The main fixes are to narrow a few broad claims about "all Hamiltonian simulation" and to avoid loose language around topological order.

## Comments

- The statement that these bounds underpin all Hamiltonian simulation algorithms is too broad. They underpin locality-sensitive simulation and lower-bound intuition; black-box, LCU, or nonlocal-oracle algorithms are not directly constrained by the same light-cone argument.

- The TQO result is about generating topological order by local Hamiltonian evolution from a state without comparable local indistinguishability. It should not be paraphrased as every topological state requiring linear time from every possible initial state.

- The correlation bound has a factor of two in the light-cone time because two evolved observables are truncated. The note gets this right; keep it explicit because it is a common source of incorrect `L/v` versus `L/(2v)` statements.

- The entanglement-growth result is an entangling-rate bound for bounded local interactions across a boundary. It does not say arbitrary finite-time dynamics preserve every useful area-law approximation property with efficient bond dimension in all geometries.

- The cross-link saying cluster states have TQO properties is potentially misleading. Ordinary cluster states are better treated as symmetry-protected or MBQC resource states, not as intrinsic topological order in the toric-code sense.

## Suggested Additions

- Add a short "assumptions" line: bounded-degree graph, bounded-strength local/short-range interactions, finite local dimension.
- Add a caveat that long-range power-law systems require different Lieb-Robinson bounds and can have non-linear light cones.
