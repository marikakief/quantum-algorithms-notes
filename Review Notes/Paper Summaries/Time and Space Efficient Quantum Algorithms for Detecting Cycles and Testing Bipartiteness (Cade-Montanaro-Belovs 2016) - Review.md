# Review: Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade-Montanaro-Belovs 2016)

Source note: [[Time and Space Efficient Quantum Algorithms for Detecting Cycles and Testing Bipartiteness (Cade-Montanaro-Belovs 2016) — Paper Notes]]

## Verdict

Minor issues. This is a strong technical note with a clear account of the mod-lift reduction and the space/time tradeoff. The revisions are mostly about input-model caveats and one array-model sentence that should be narrowed to cycle detection.

## Line-Anchored Comments

- Lines 12-30: Good separation of adjacency-matrix and adjacency-array models. Keep the statement that the oracle is not charged against working-space; that assumption matters.

- Lines 32-52: The main results are clear. For the array model, emphasize that the result is not query-optimal in all degree regimes; it buys logarithmic space.

- Lines 55-81: The mod-3 lift construction is well explained. Add a short proof sketch for why acyclic graphs cannot connect `s` to `t` in the lift.

- Lines 83-93: The random orientation repair is excellent. Note that pairwise independence is enough because only the two neighbors of the chosen vertex on the cycle are being compared.

- Lines 95-113: The local short-cycle test should specify whether the Belovs-Reichardt subroutine's input graph is given by an adjacency oracle computed from `G`; the graph `H` is implicit.

- Lines 144-159: "If `m >= n` the graph has a cycle and can be rejected/accepted accordingly" applies to cycle detection in an undirected graph. It is not enough for bipartiteness, because the cycle may be even. Clarify this.

- Lines 171-177: The technical assessment is accurate. The value is the time-space implementation, not a new query exponent.

## Missing or Suggested Additions

- Add a small schematic of the mod-2 versus mod-3 lift roles: odd-cycle detection versus general-cycle detection.
- Add a note that the algorithm uses bounded-error quantum subroutines whose amplification must preserve the space bound.
