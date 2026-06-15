# Review: A Scheme for Efficient Quantum Computation with Linear Optics (Knill-Laflamme-Milburn 2001)

Source note: [[A Scheme for Efficient Quantum Computation with Linear Optics (Knill-Laflamme-Milburn 2001) — Paper Notes]]

## Primary Sources Checked

- Knill, Laflamme, and Milburn, "A scheme for efficient quantum computation with linear optics," Nature 409, 46-52 (2001); arXiv:quant-ph/0006088.

## Verdict

Minor issues. The note captures the central surprise: measurement-induced nonlinearity plus teleportation makes passive linear optics universal. The main improvement is to keep the theoretical scalability claim separate from the very large practical resource overhead.

## Line-Anchored Comments

- Lines 7-16: good summary. "No optical nonlinearity required" should be read as no physical Kerr-type nonlinearity; photon counting and postselection supply the effective nonlinearity.
- Lines 22-28: dual-rail encoding and single-qubit linear optics are described correctly.
- Lines 32-49: the `NS_1` and nondeterministic c-sign construction is clear. The success probabilities `1/4` and `1/16` are the right headline numbers.
- Lines 52-75: the gate-teleportation discussion is good. Detected failure is the important fault-tolerance distinction; keep that phrase near the success probability.
- Lines 58-64: the `|t_n>` resource should be explicitly normalized if the formula is copied into a standalone note.
- Lines 78-91: the subexponential resource-state preparation bound is useful, but the relationship to "efficient" computation is subtle. In the fault-tolerant architecture, one chooses teleportation/error-correction parameters to make the total overhead polynomial; the raw `2^{O(sqrt n)}` state-preparation bound by itself is not the whole efficiency statement.
- Lines 97-101: "polynomial once the teleportation parameter exceeds threshold" is a little too quick. Threshold arguments require concatenated/error-detecting codes and accounting for resource-state preparation, not merely choosing `n <= 25`.
- Lines 118-124: good practical caveats. Photon loss and detector imperfections are not small implementation details; they change the error model away from the clean detected-erasure setting.

## Missing or Suggested Additions

- Add a compact distinction between postselected gates, teleportation-boosted near-deterministic gates, and fault-tolerant scalable LOQC.
- Add a dated-resource warning: later fusion/cluster-state photonic schemes supersede KLM's raw overhead while preserving the measurement-induced-nonlinearity idea.

