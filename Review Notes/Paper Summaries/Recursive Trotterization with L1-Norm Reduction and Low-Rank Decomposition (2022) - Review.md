# Review: Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022)

Source note: [[Recursive Trotterization with L1-Norm Reduction and Low-Rank Decomposition (2022) — Paper Notes]]

## Primary Sources Checked

- Low, Su, Tong, Tran, "On the complexity of implementing Trotter steps," PRX Quantum 4, 020323 (2023) / arXiv:2211.09133.

## Verdict

Major omissions. The note identifies the right bottleneck, but it is too compressed for a technically important paper and the filename does not match the actual title.

## Line-Anchored Comments

- Lines 2 and 8-16: the bibliographic title is "On the complexity of implementing Trotter steps." The current vault title foregrounds only part of the contribution and may mislead future searches.
- Lines 10-16: good framing: Trotter error bounds and Trotter-step implementation costs are different problems.
- Lines 20-26: "sublinear in `n` for appropriate `alpha` regimes" and "roughly `O(rho n polylog)`" are too vague. This paper is about gate-complexity theorems; the note should state the regimes and parameters precisely.
- Lines 28-32: the uniform-electron-gas expression needs the assumptions and suppressed factors. Say whether this is total gate complexity, per-step cost, or combined Trotterized simulation complexity.
- Lines 34-36: the lower bound is important and should name the model more carefully: generic commuting 2-local Hamiltonians do not admit subquadratic implementation without additional structure.
- Lines 44-48: only two reusable tricks are extracted. The average-cost implementation idea deserves its own card or at least a fuller explanation.

## Missing or Suggested Additions

- Add a theorem table separating power-law interactions, low-rank coefficient blocks, average-cost simulation, uniform electron gas, and the commuting-2-local lower bound.
- Add a note that this is not primarily an error-analysis paper; it is about compiling a Trotter step faster than the naive term-by-term implementation.
