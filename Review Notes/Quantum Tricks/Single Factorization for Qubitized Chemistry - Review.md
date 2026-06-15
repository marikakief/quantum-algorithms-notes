# Review: Single Factorization for Qubitized Chemistry

Source note: [[Single Factorization for Qubitized Chemistry]]

## Primary Sources Checked

- Berry et al., arXiv:1902.02134.
- Motta et al., arXiv:1808.02625 for low-rank/double-factorization background.

## Verdict

Minor issues. The main idea is right and the card usefully emphasizes chemist ordering. The caveats should be moved closer to the headline scaling.

## Line-Anchored Comments

- Line 6: the total phase-estimation complexity should include `/ Delta E`; the card later does so at line 33. The opening sentence should match.
- Line 13: `L = O(N)` is empirical/physically motivated, not guaranteed for all arbitrary molecular bases. This is mentioned later only indirectly.
- Lines 17-22: the `O(N^3)` coefficient count and QROAM `O(N^{3/2})` cost story is good. It should specify that `M`/output precision only contributes polylog or square-root factors in the simplified statement.
- Line 24: the chemist-vs-physicist ordering warning is excellent and should remain prominent.
- Lines 31-34: good complexity summary, but "space `O(N)` dirty ancilla variant" should note that system qubits are being reused as dirty ancillae subject to compatibility constraints.
- Line 37: the `lambda_W >= lambda_V` point is important. It would be clearer to say this is an LCU 1-norm tax caused by factorizing before taking absolute values.
- Line 39: "THC effectively achieves a version of this second factorization" is suggestive but imprecise. THC is a different tensor ansatz, not simply the second eigenvalue truncation from Motta et al. transplanted into LCU.

## Missing or Suggested Additions

- Add a small "not double factorization" warning: single factorization is the first Coulomb-supermatrix decomposition only; double factorization adds eigenvalue truncations of each factor.

