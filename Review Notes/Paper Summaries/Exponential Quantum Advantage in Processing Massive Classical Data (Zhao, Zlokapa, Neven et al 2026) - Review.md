# Review: Exponential Quantum Advantage in Processing Massive Classical Data (Zhao, Zlokapa, Neven et al 2026)

Source note: [[Exponential Quantum Advantage in Processing Massive Classical Data (Zhao, Zlokapa, Neven et al 2026) — Paper Notes]]

## Primary Sources Checked

- Zhao, Zlokapa, Neven, Babbush, Preskill, McClean, and Huang, "Exponential quantum advantage in processing massive classical data," arXiv:2604.07639 (submitted April 8, 2026).

## Verdict

Minor issues. The note is careful and does not overclaim the headline. The main improvements are to keep the bounded-space streaming model visible in every advantage statement and to mark the application theorems as parameter-regime results, not generic wins for all classification or PCA.

## Line-Anchored Comments

- Lines 9-10: excellent opening. "No qRAM" is an important distinction and should remain near the title-level summary.
- Lines 26-34: the machine-size/sample-complexity framing is good. Say explicitly that the exponential advantage is usually "polylog-sized quantum machine versus polynomial-sized classical memory," i.e. exponential in the machine-size parameter, not necessarily exponential in the raw dataset dimension phrased as time.
- Lines 40-52: the oracle-sketching explanation is useful. The phase normalization and lower-bound formula are delicate; avoid reusing the displayed `p(x) f(x) t` and `p_max t^2 Q^2 / epsilon` expressions without checking the exact theorem statement.
- Lines 62-64: the randomized Hadamard flattening point is good and deserves to stay; it is one of the least slogan-like contributions.
- Lines 74-84: good readout caveat. The classical shadow artifact answers a specified family of sparse test queries with its own sample/observable dependence; it is not a full classical model vector.
- Lines 96-100: the NOPE lower-bound statement should remain attached to its oracle-separation hypotheses. Do not let it read as a direct lower bound for arbitrary real-world datasets.
- Lines 104-116: the detailed theorem caveats are valuable. The line `D^{1-chi}` should be checked against the paper's exact definition of guiding-vector quality before being quoted elsewhere.
- Lines 130-138: strong limitations section. The distinction between static same-sample space lower bounds and dynamic superpolynomial sample separations is especially important.
- Lines 142-148: the assessment is balanced. Keep the phrase "inside the model they define"; it is the right epistemic status.

## Missing or Suggested Additions

- Add a status label: "2026 v1; streaming-sample/bounded-space separation; not a blanket practical QML advantage."
- Add a parameter table for the clean `tilde O(N)` statements: sparsity, condition number, margin, gap, repetition number, and test-query assumptions.

