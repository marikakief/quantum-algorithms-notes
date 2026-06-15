# Review: QROM (Quantum Read-Only Memory)

Source note: [[QROM (Quantum Read-Only Memory)]]

## Primary Sources Checked

- Babbush et al., arXiv:1805.03662, Section III.C.
- Berry et al., arXiv:1902.02134, Appendix C for later measurement-based uncomputation.

## Verdict

Major issues. The QROM construction is basically right, but the card incorrectly folds later measurement-based uncomputation into the original QROM primitive and is sloppy about T versus Toffoli accounting.

## Line-Anchored Comments

- Line 6: `4L - 4` should be source-labeled as the unary-iteration T-gate accounting used in Babbush et al. 2018. Later chemistry papers generally quote QROM/QROAM in Toffoli counts, so the unit matters.
- Lines 14-18: the core fixed-table lookup description is good. It correctly distinguishes compiled QROM from dynamic hardware QRAM.
- Line 20: "uncompute by measure and classically correct" is not part of the original QROM primitive in the 2018 PRX paper. It is the 2019 QROAM/measurement-uncomputation improvement and requires the output register to be consumed in a compatible way.
- Line 24: T-count independence from word size is right only for the data-loading CNOT layer. Clifford count, routing, and measurement-uncompute costs can still scale with word size or with the fixup table.
- Lines 35-38: listing output uncomputation as "0 additional T gates" is misleading for a generic QROM lookup. Reversal costs another QROM; measurement-based cleanup has a different cost model and preconditions.
- Line 43: "acceptable here because SELECT also costs `O(N)`" is internally odd when the example table has `L=O(N^2)`. Say instead that the 2018 dual-basis construction arranges the relevant table sizes and SELECT/PREPARE costs so the total oracle is linear in the number of spin orbitals in the final theorem.
- Lines 54-55: the SelectSwap/LKS relation should use the original arXiv year 2018 unless the note is deliberately citing the later journal version.

## Missing or Suggested Additions

- Split this into "QROM lookup", "QROM reversal", and "measurement-based QROM output cleanup" as three separate cost lines.
- State explicitly that QROM is best for classical read-only tables fixed at compile time.

