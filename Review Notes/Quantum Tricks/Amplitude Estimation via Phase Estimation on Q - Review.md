# Review: Amplitude Estimation via Phase Estimation on Q

Source note: [[Amplitude Estimation via Phase Estimation on Q]]

## Primary Sources Checked

- BHMT, "Quantum Amplitude Amplification and Estimation": https://arxiv.org/abs/quant-ph/0005055
- BHT, "Quantum Counting": https://arxiv.org/abs/quant-ph/9805082

## Verdict

Minor issues. The core theorem and mechanism are right; the counting specialization needs a better cost statement.

## Line-Anchored Comments

- Lines 7-13: mechanism

  Correct. `Q` has eigenphases `±2 theta_a` on the good/bad two-dimensional invariant subspace, and QPE yields `a = sin^2(theta_a)`.

- Lines 15-21: theorem

  Correct standard BHMT error bound with success probability `8/pi^2`.

- Line 25: "Total: O(sqrt(N)/epsilon) for estimating t within epsilon N"

  This is not the cleanest way to state it. Additive error `epsilon` in `a=t/N` costs `O(1/epsilon)` Grover-operator uses, independent of `N` except through oracle/preparation cost. If the goal is additive error `epsilon N` in `t`, the query count is `O(1/epsilon)`, not `O(sqrt(N)/epsilon)`. The `O(sqrt(N))` regime corresponds to `epsilon = 1/sqrt(N)` or errors on the order of `sqrt(tN)`.

- Lines 31-32: "any projector"

  Fine if a coherent reflection/check for the projector is available. Add that implementing `S_chi` or a projector reflection can be the dominant cost.

- Lines 39-40: alternatives

  Kaiser-window AE and other modern variants do not remove the need for coherent access entirely in every setting; say they modify the spectral window/constants rather than simply reducing overhead.

## Missing or Suggested Additions

- Add a table separating additive error in `a`, additive error in `t`, and relative error in `t`.
