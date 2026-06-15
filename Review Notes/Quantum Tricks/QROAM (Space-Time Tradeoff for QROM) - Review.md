# Review: QROAM (Space-Time Tradeoff for QROM)

Source note: [[QROAM (Space-Time Tradeoff for QROM)]]

## Primary Sources Checked

- Berry et al., arXiv:1902.02134, Appendix C.
- Low, Kliuchnikov, Schaeffer, arXiv:1812.00954.

## Verdict

Major issues. The clean-ancilla story is mostly right, but the dirty-ancilla table contains a likely erroneous cost expression and the lower-bound claim is too broad.

## Line-Anchored Comments

- Line 1: the source attribution is confusing: Low-Kliuchnikov-Schaeffer is arXiv:1812.00954, not intrinsically a 2024 result unless citing a later publication. Use arXiv year for historical chronology.
- Lines 10-15: the clean-ancilla page-and-swap explanation is good. The `ceil(d/k)+M(k-1)` cost is the right object to optimize.
- Lines 17-24: the dirty-ancilla description is plausible, but "the swapping network must be repeated twice (with Hadamards between)" needs either a figure reference or a clearer invariant; otherwise it is hard to audit.
- Line 37: the clean uncompute cost `~2 sqrt(d)` is measurement-based output cleanup, not ordinary inverse-QROAM uncomputation. Keep that qualification in the table.
- Line 38: the dirty-row formulas are suspect. `2dM/N + O(N)` for compute and `~dN/4` for uncompute do not follow from the earlier `2 ceil(d/k)+4M(k-1)` / `2 ceil(d/k)+4k` forms without defining how `k`, `M`, and available dirty qubits relate. This row should be recomputed from the paper's formulas.
- Line 41: "4x more expensive" is not a universal dirty-vs-clean statement; it depends on table width, available dirty qubits, and chosen `k`.
- Line 43: the lower-bound sentence is too strong. LKS provides lower bounds for their general state-preparation/unitary-synthesis tradeoff model; do not claim no future QROM variant can asymptotically beat `sqrt(dM)` without specifying the oracle/model assumptions.

## Missing or Suggested Additions

- Add a worked example with `d`, `M`, and available dirty qubits to make the tradeoff concrete.
- Use "Toffoli" consistently; do not mix with T-gate language inherited from the QROM card.

