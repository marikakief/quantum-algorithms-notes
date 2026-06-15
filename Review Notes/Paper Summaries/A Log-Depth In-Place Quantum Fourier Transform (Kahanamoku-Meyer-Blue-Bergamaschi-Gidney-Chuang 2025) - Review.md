# Review: A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025)

Source note: [[A Log-Depth In-Place Quantum Fourier Transform (Kahanamoku-Meyer-Blue-Bergamaschi-Gidney-Chuang 2025) — Paper Notes]]

## Primary Sources Checked

- Kahanamoku-Meyer et al., "A log-depth in-place quantum Fourier transform that rarely needs ancillas", arXiv:2505.00701. Checked current arXiv metadata and abstract.

## Verdict

Major issues. The note captures the optimistic-circuit framework well, but it incorrectly calls the optimistic QFT nearest-neighbour in places. The source claims logarithmic locality/range, not strictly nearest-neighbour gates.

## Line-Anchored Comments

- Lines 13-15: replace "nearest-neighbour gates in 1D" with "logarithmic gate range/locality in a 1D layout." The note's own line 75 says gate range is `O(log(n/epsilon))`.
- Lines 21-28: the Frobenius-norm/optimistic definition is well explained.
- Lines 31-44: the block construction is useful, but there is an internal denominator inconsistency. Line 33 writes an inter-block phase with `/2^m`, while lines 39 and 41 use `/2^{2m}`. Recheck the paper's formula and make the convention consistent.
- Lines 53-64: the worst-to-average-case reduction is clear. Emphasize that the randomized reduction gives expected error; the purified version is deterministic but spends qubits.
- Lines 66-70: the Shor/factoring discussion is good, but say the optimistic QFT is justified by the distribution of intermediate values, not by worst-case QFT correctness.
- Lines 91-99: update the comparison table row from "NN in 1D" to logarithmic range. Otherwise the table overclaims the central result.
- Lines 103-111: the caveats are strong and should remain.

## Missing or Suggested Additions

- Add one sentence distinguishing three guarantees: optimistic average/Frobenius error, randomized worst-input expected error, and unitary derandomized worst-input error.

