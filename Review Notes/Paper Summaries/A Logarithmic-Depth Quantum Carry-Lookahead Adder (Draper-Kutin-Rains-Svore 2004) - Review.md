# Review: A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004)

Source note: [[A Logarithmic-Depth Quantum Carry-Lookahead Adder (Draper-Kutin-Rains-Svore 2004) — Paper Notes]]

## Primary Sources Checked

- Draper, Kutin, Rains, and Svore, "A logarithmic-depth quantum carry-lookahead adder", arXiv:quant-ph/0406142 / Quantum Information and Computation 6, 351-369 (2006).

## Verdict

Minor issues. The note is detailed and mostly correct. The main comparison issue is the QFT-adder depth row.

## Line-Anchored Comments

- Lines 14-16: good summary of why reversible carry-lookahead is nontrivial.
- Lines 33-39: the propagate/generate recurrence is clearly stated.
- Lines 53-57: the overlapping schedule is useful. Keep the exact depth formula if it matches the source's assumptions on parallel gates.
- Lines 67-87: the resource formulas are detailed and internally consistent.
- Lines 91-99: the modular/comparison extensions are a valuable inclusion.
- Line 121: saying the Draper QFT adder has `O(log n)` approximate depth is too compressed. The addition rotations can be parallelized, but the full QFT-adder depth depends on the QFT implementation and any ancillae/connectivity used.
- Line 126: "QCLA's `O(n)` Toffolis (= `O(n)` T gates with temporary logical-ANDs)" needs care. Temporary logical-AND accounting is constant-factor T cost, but the exact T count depends on which Toffolis come in compute/uncompute pairs and the chosen tree.
- Lines 130-138: the caveats are good and should stay.

## Missing or Suggested Additions

- Add a small note that QCLA optimizes depth at significant ancilla cost; Gidney-style ripple approaches optimize T count/workspace at linear depth.

