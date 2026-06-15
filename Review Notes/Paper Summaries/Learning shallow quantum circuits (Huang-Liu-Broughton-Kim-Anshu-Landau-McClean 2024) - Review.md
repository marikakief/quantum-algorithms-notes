# Review: Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024)

Source note: [[Learning shallow quantum circuits (Huang-Liu-Broughton-Kim-Anshu-Landau-McClean 2024) — Paper Notes]]

## Primary Sources Checked

- Huang et al., "Learning shallow quantum circuits", arXiv:2401.10095 / STOC 2024.

## Verdict

Major issues. The high-level explanation is good, but the sewing identity appears to omit normalization/local-Pauli details. That is central enough to fix.

## Line-Anchored Comments

- Lines 20-24: the conceptual separation between learning and sampling is well stated.
- Lines 30-36: the local Heisenberg observable description is good. For arbitrary constant-depth two-qubit circuits, the lightcone is constant-size because depth is constant.
- Lines 42-44: likely missing the swap normalization. The local swap identity has a factor such as `1/2 sum_P P \otimes P` for one qubit. A formula for `W_i` as a raw sum over Pauli labels is not unitary as written.
- Lines 56-64: the global sewing identity should be checked against the paper. The ordering, global swap placement, and normalization of local factors are delicate; if this formula is wrong, it misteaches the core trick.
- Lines 85-99: the sample complexity and learned-dilation description are useful. State whether this is for arbitrary architecture or a geometrically local refinement.
- Lines 121-123: good hardness-boundary point. Add "in general" and the exact depth regime from the paper.
- Lines 136-140: the caveats are good and should be kept.

## Missing or Suggested Additions

- Add a corrected two-line derivation of the local swap/conjugated-swap identity. This note depends on that identity more than on any resource table.

