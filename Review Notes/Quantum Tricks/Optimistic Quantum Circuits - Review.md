# Review: Optimistic Quantum Circuits

Source note: [[Optimistic Quantum Circuits]]

## Primary Sources Checked

- Kahanamoku-Meyer et al., arXiv:2505.00701.

## Verdict

Major issues. The framework is described well, but the QFT resource bullet repeats the nearest-neighbor overclaim.

## Line-Anchored Comments

- Lines 12-22: good definition and bad-subspace bound.
- Line 24: the Shor drop-in claim should say "small expected error/success degradation under the paper's distributional argument," not simply that all intermediate values are uniformly random in every implementation.
- Lines 36-38: replace "nearest-neighbour in 1D" with "logarithmic gate range/locality in 1D." This is the same issue as the paper summary.
- Lines 42-46: the caveats are good. The diamond-distance warning is especially useful.

## Missing or Suggested Additions

- Add a short warning that optimistic error is not an input-distribution claim chosen by the user; it is a Frobenius-average statement over Hilbert space, and using it in an algorithm still requires a distributional or reduction argument.

