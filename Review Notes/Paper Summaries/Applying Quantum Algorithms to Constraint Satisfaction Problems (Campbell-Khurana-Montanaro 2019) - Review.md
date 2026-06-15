# Review: Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019)

Source note: [[Applying Quantum Algorithms to Constraint Satisfaction Problems (Campbell-Khurana-Montanaro 2019) — Paper Notes]]

## Primary Sources Checked

- Campbell, Khurana, and Montanaro, "Applying quantum algorithms to constraint satisfaction problems," Quantum 3, 167 (2019); arXiv:1810.05582.

## Verdict

Minor issues. The note is strong and gets the paper's main lesson right: asymptotic square-root speedups are not enough; full fault-tolerant overhead and classical decoder cost can dominate.

## Line-Anchored Comments

- Lines 9-15: good statement of the benchmark setting. The random-instance/threshold restriction should remain near all speedup tables.
- Lines 20-24: excellent high-level assessment. The paper is more a resource-estimation methodology than a near-term CSP breakthrough.
- Lines 32-40: Grover oracle details are clear. The Zalka iteration count is worth keeping because failure probability assumptions matter in resource estimates.
- Lines 44-63: the backtracking implementation details are good. The exact-amplitude-amplification diffusion step should be cross-checked against the BHMT phase-adjusted version if copied into a standalone trick card.
- Lines 71-85: speedup tables are useful. Keep hardware-regime labels and one-day-runtime assumptions beside the numbers.
- Lines 87-91: classical benchmark details are essential. These estimates age quickly; preserve the solver names, year, and fitted exponents.
- Lines 104-110: the Grover-vs-backtracking lesson is one of the best parts of the note.
- Lines 114-123: the classical surface-code decoding overhead warning is crucial. This is not a minor implementation constant.
- Lines 129-137: caveats are good. Add that CDCL-style learning is a major classical advantage for SAT and is not captured by simple backtracking tree-size comparisons.

## Missing or Suggested Additions

- Add a status/date warning: resource estimates depend on 2019 surface-code factory and decoder assumptions.
- Add a small "detection vs search" line in the key-results section, not only in caveats.

