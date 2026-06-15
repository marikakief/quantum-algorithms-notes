# Review: Quantum Walk on Backtracking Trees via Effective Resistance

Source note: [[Quantum Walk on Backtracking Trees via Effective Resistance]]

## Primary Sources Checked

- Montanaro, arXiv:1509.02374: https://arxiv.org/abs/1509.02374
- Belovs, arXiv:1302.3143, for quantum walks and electric networks.

## Verdict

Sound with minor caveats. The construction and complexity are correctly stated. The main improvement would be to define the tree parameters and branch-degree assumptions more explicitly.

## Line-Anchored Comments

- Line 8: "tree is defined implicitly"

  Good. This is the central distinction from MNRS-style search on an explicitly known Markov chain.

- Line 12: local diffusion construction is right.

  For implementation, each reflection requires computing children according to the classical heuristic `h` and predicate `P`. The card should mention that reversible implementation cost is hidden in the evaluation oracle.

- Lines 14-18: marked-case eigenvector is correct for a marked vertex/path.

  The overlap lower bound relies on the root weighting and the tree depth bound `n`. If `n` is replaced by a different depth upper bound, the weighting and cost should track that parameter.

- Line 22: effective-resistance statement is useful.

  Add that for trees the effective resistance from root to a marked vertex is at most the depth, hence the `sqrt(Tn)` bound.

- Line 35: finding complexity is correct for constant branching/domain.

  If domain size `d` is not constant, the child-subtree search phase has a `d` dependence.

- Line 39: caveat is good.

  "Always explores the whole tree" should be softened: the detection subroutine is analyzed against an upper bound on the whole tree size, so early classical success can beat the quantum method on some instances.

## Missing or Suggested Additions

- Add definitions: `T` = tree-size upper bound, `n` = maximum depth/number of CSP variables, `delta` = allowed failure probability.
