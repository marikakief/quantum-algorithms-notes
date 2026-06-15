# Review: Quantum Pattern Matching Fast on Average (Montanaro 2015)

Source note: [[Quantum Pattern Matching Fast on Average (Montanaro 2015) — Paper Notes]]

## Primary Sources Checked

- Montanaro, "Quantum pattern matching fast on average," Algorithmica 77, 16-39 (2017), arXiv:1408.1816.

## Verdict

Minor issues. The note is careful and correctly resists overselling the algorithm as practical string matching. The main edits are small model/indexing clarifications.

## Line-Anchored Comments

- Lines 9-21: good problem statement. Depending on indexing convention, the offset set should be `[n-m+1]^d` rather than `[n-m]^d`; clarify whether the notation is zero-based half-open.
- Lines 25-28: injectivity and local injectivity are well defined. Keep the separation parameter `gamma` attached to the structured theorem.
- Lines 32-40: excellent high-level assessment. The hidden-shift factor is genuinely the important caveat.
- Lines 46-58: the injectivization-by-blocks explanation is clear and useful.
- Lines 62-79: good hidden-shift discussion. Note that the subroutine's cost is query complexity plus substantial quantum memory/classical processing for a Kuperberg-type sieve.
- Lines 83-104: the RoughCheck/coarse-offset structure is clear.
- Lines 108-125: the average-case theorem is well stated. Make explicit that the failure probability is over both the random instance model and algorithmic randomness.
- Lines 127-134: the structured theorem is complex but worth keeping; it prevents the average-case theorem from sounding like magic.
- Lines 177-181: caveats are strong. "qRAM-style access" should be softened to "oracle access"; the paper's formal model is query access, not necessarily a data-structure proposal.

## Missing or Suggested Additions

- Add a one-line comparison to worst-case quantum string matching: this paper is faster only in the average/promise regime; worst-case verification remains expensive.

