# Review: On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers (Geraci-Lidar 2007)

Source note: [[On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers (Geraci-Lidar 2007) — Paper Notes]]

## Primary Sources Checked

- Geraci and Lidar, "On the Exact Evaluation of Certain Instances of the Potts Partition Function by Quantum Computers," arXiv:quant-ph/0703023; Commun. Math. Phys. 279, 735 (2008).

## Verdict

Minor issues. The note is careful about the narrow graph family and the exactness mechanism. It is a useful contrast to the additive-approximation Temperley-Lieb/Tutte algorithms.

## Line-Anchored Comments

- Lines 11-18: good problem statement. "Fully ferromagnetic and fully anti-ferromagnetic" should be tied to uniform coupling and the paper's chosen parameterization of `y`.
- Lines 19-43: the `ICCC_epsilon` conditions are appropriately explicit. This is a highly engineered family, and the note does not hide that.
- Lines 47-53: strong high-level assessment. Exactness at physical real-temperature parameters is the selling point, but the price is a severe code-theoretic restriction.
- Lines 59-73: graph-to-cocycle-code reduction is clear.
- Lines 75-101: membership testing via discrete logs is well explained. Mention that the recognition step itself is quantum in the stated method, not a free promise unless the input is already certified.
- Lines 113-139: McEliece/Gauss-sum route is detailed enough. The use of phase estimation should always be paired with the later rounding/divisibility step.
- Lines 141-155: the exact-weight recovery explanation is excellent. This is the key trick that turns approximate Gauss phases into exact integer weights.
- Lines 169-190: the MacWilliams identity paragraph is dense. Recheck notation before reusing the displayed formula, since the paper's `A`/`A^\perp` convention can easily be swapped.
- Lines 203-215: good note about the bracketing typo in the success probability.
- Lines 248-255: caveats are strong and should remain near any speedup claim.

## Missing or Suggested Additions

- Add a one-line "promise vs recognition" distinction: exact evaluation is for graphs in `ICCC_epsilon`; testing that condition uses quantum discrete logs.
- Add a note that the speedup is against the cited best-known classical route, not an unconditional lower bound.

