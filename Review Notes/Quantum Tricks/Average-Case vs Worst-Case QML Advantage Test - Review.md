# Review: Average-Case vs Worst-Case QML Advantage Test

Source note: [[Average-Case vs Worst-Case QML Advantage Test]]

## Primary Sources Checked

- Huang, Kueng, and Preskill, arXiv:2101.02464 / Physical Review Letters 126, 190505 (2021).

## Verdict

Minor issues. This is a strong reusable sanity-check card.

## Line-Anchored Comments

- Lines 12-28: the two-regime distinction is correct and useful.
- Lines 18-22: define `m` as output-qubit count or observable support dimension before using the average-case query overhead.
- Lines 28-35: the worst-case Pauli separation should say "query/copy separation with quantum memory" rather than a practical ML advantage.
- Lines 42-44: excellent caveat about query/sample versus runtime.

## Missing or Suggested Additions

- Add a third category: cryptographic/computational-assumption separations. The HKP theorem constrains information-theoretic average-case claims, not every possible computational QML claim.

