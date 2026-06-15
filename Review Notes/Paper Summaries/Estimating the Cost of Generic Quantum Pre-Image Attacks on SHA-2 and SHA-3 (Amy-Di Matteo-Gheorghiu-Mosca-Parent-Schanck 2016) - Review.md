# Review: Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016)

Source note: [[Estimating the Cost of Generic Quantum Pre-Image Attacks on SHA-2 and SHA-3 (Amy-Di Matteo-Gheorghiu-Mosca-Parent-Schanck 2016) — Paper Notes]]

## Primary Sources Checked

- Amy, Di Matteo, Gheorghiu, Mosca, Parent, and Schanck, "Estimating the cost of generic quantum pre-image attacks on SHA-2 and SHA-3", arXiv:1603.09383 (2016).

## Verdict

Minor issues. This is a careful summary of a cost-model paper. The note correctly resists the slogan "Grover halves security"; the main recommendation is to flag architectural assumptions each time the `2^166` figures are quoted.

## Line-Anchored Comments

- Lines 17-29: good reversible Grover oracle framing.
- Lines 33-37: excellent statement of logical-qubit-cycles. The "one surface-code cycle equals one classical hash invocation" assumption should be repeated near the headline result.
- Lines 41-43: good assessment. The estimates are historical and model-dependent, not lower bounds.
- Lines 66-82: the Lambert-`W` overhead calculation is a valuable addition. It explains why finite-key overhead matters even when the query exponent is `k/2`.
- Lines 90-103: SHA-256 circuit ingredients look right. The note should mention that adders dominate depth differently from Toffoli count after T-par optimization.
- Lines 111-128: the SHA3 discussion is strong. The T-depth/T-width contrast is the main reason SHA3 has lower cycles but many more distilleries.
- Lines 132-156: good distillation assumptions. These should be tagged as assumptions from the paper's era, not general surface-code facts.
- Lines 160-229: the arithmetic for `T^c_G`, cycles, and logical-qubit-cycles is useful. Keep query count, T-count, T-depth, cycles, logical qubits, and physical qubits as separate rows; do not collapse them into one "cost".
- Lines 261-266: excellent limitations. The "not a lower bound" caveat should be near the top as well.

## Missing or Suggested Additions

- Add a sentence explaining that generic preimage search assumes the hash is random-like and does not model structural SHA cryptanalysis.
- When comparing SHA-256 and SHA3-256, say explicitly that their near-equal area-time estimates arise from a tradeoff: SHA3 has much lower T-depth but needs many more T factories.

