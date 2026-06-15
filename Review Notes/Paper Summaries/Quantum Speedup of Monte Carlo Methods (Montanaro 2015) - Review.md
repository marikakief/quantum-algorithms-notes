# Review: Quantum Speedup of Monte Carlo Methods (Montanaro 2015)

Source note: [[Quantum Speedup of Monte Carlo Methods (Montanaro 2015) — Paper Notes]]

## Primary Sources Checked

- Montanaro, "Quantum speedup of Monte Carlo methods," Theory of Computing 11, 1-36 (2015), arXiv:1504.06987.

## Verdict

Minor issues. The note captures the main algorithm and the partition-function application well. The biggest risk is that readers may miss that the speedup is in coherent calls to the sampling subroutine, including its inverse and state-preparation cost.

## Line-Anchored Comments

- Lines 9-16: good statement of the additive mean-estimation problem. Say whether `sigma` is known or supplied as an upper bound; the algorithmic cost depends on such a bound.
- Lines 21-23: "general-purpose" is fine if immediately qualified by coherent access to `A` and `A^{-1}`. This is not a black-box speedup for arbitrary irreversible simulation code.
- Lines 31-38: good amplitude-estimation explanation. The error expression `O(sqrt(mu)/t + 1/t^2)` is specific to bounded nonnegative outputs; keep the bounded-output precondition attached.
- Lines 43-49: the centering-and-dyadic-splitting description is clear. Add that the logarithmic factors come from truncation/range splitting and boosting, not from amplitude estimation alone.
- Lines 55-60: check the dependence on the relative-variance parameter before reusing the table. In the paper's Algorithm 4 the dependence on `B` is not a clean quadratic improvement in every regime; the clean near-quadratic speedup emphasized in the abstract is for the accuracy parameter and for bounded-variance mean estimation.
- Lines 72-80: the partition-function cost line should preserve the extra `sqrt(tau)`/state-preparation term. The simplified table at line 111 drops the additive `sqrt(tau)` contribution visible at line 80.
- Lines 96-100: excellent caveat about coherent Gibbs-state preparation. This is where many sloppy summaries of the paper fail.
- Lines 119-124: strong limitations section. The Herbert caveat is useful but should be labelled as a later critique of a particular state-preparation route, not a flaw in Montanaro's theorem.
- Line 113: "All bounds are optimal up to polylog factors" is too broad. The `Omega(1/epsilon)` lower bound applies to mean estimation; application-level partition-function bounds also depend on the Markov-chain and schedule assumptions.

## Missing or Suggested Additions

- Add a compact resource-model box: coherent implementation of `A`, ability to reflect/phase-estimate, inverse calls, known variance or relative-variance bounds, and state-preparation cost counted inside `A`.

