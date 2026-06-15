# Review: Classical Preprocessing plus Grover Search

Source note: [[Classical Preprocessing plus Grover Search]]

## Primary Sources Checked

- Brassard, Hoyer, and Tapp, "Quantum Algorithm for the Collision Problem", arXiv: https://arxiv.org/abs/quant-ph/9705002
- ar5iv rendering: https://ar5iv.labs.arxiv.org/html/quant-ph/9705002

## Verdict

Minor issues. The collision-search template is right. The QRAM/table-access claim needs a model caveat.

## Line-Anchored Comments

- Lines 7 and 11-13: birthday plus Grover balance

  Correct for the BHT collision algorithm under an `r`-to-one promise: table cost `k`, Grover cost about `sqrt(N/(r k))`, optimum `k = (N/r)^{1/3}`.

- Line 12: number of solutions

  Use `Theta((r-1)k)` when the initial table has no internal collisions and the function is exactly `r`-to-one. For arbitrary functions or tables with repeated images, this count changes and BHT modifies the sampling assumptions.

- Line 15: quantum-accessible table

  This is a good caveat, but "QRAM or quantum-readable classical memory" needs care. In the query model, table lookup/sorting can be treated separately; in a circuit/resource model, embedding table lookup into the Grover predicate can dominate constants or require nontrivial QROM/QRAM assumptions.

- Lines 26-28: tradeoff

  Present `ST^2 >= N` as the scaling relation obtained by this algorithmic family, not as a lower bound. The companion `Birthday-Grover Space-Time Tradeoff` card currently overstates this.

- Lines 32-34: element distinctness

  Correct. Without the `r`-to-one promise, the problem is element distinctness and the tight query complexity is `Theta(N^{2/3})`, not `N^{1/3}`.

## Missing or Suggested Additions

- Add a note that sorting and binary search are free only in a pure oracle-query headline; actual runtime includes reversible comparison/table-lookup cost.
