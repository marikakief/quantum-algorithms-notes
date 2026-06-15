# Review: Classical shadows (Huang-Kueng-Preskill 2020)

Source note: [[Classical shadows (Huang-Kueng-Preskill 2020)]]

## Primary Sources Checked

- Huang, Kueng, and Preskill, "Predicting many properties of a quantum system from very few measurements", arXiv:2002.08953 / Nature Physics 16, 1050-1057 (2020).

## Verdict

Major issues. This is explicitly a stub, and the one-line sample-complexity claim is too broad without the shadow-norm dependence.

## Line-Anchored Comments

- Line 3: fine to mark as a stub, but because many later notes link here, this stub should not contain oversimplified claims.
- Lines 21-25: the protocol description is basically right.
- Line 23: `O(log M / epsilon^2)` for `M` arbitrary linear observables is incomplete. Classical shadows give bounds involving the maximum shadow norm or related variance quantity for the chosen measurement ensemble. Arbitrary global observables can have bad dependence even if the number of observables enters logarithmically.
- Line 24: "better than shadow tomography for local observables; similar or worse for global ones" is directionally right, but should specify measurement ensemble: local Clifford, global Clifford, and other ensembles behave differently.
- Lines 37-41: the TODO list is appropriate. Add "state the shadow norm theorem" as the first TODO.

## Missing or Suggested Additions

- Add the measurement channel `M`, inverse channel `M^{-1}`, unbiased estimator, and median-of-means recipe. This card is a hub for too many later notes to stay this skeletal.

