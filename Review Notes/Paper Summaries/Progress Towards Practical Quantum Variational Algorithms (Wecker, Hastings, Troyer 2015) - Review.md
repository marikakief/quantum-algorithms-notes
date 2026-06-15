# Review: Progress Towards Practical Quantum Variational Algorithms (Wecker-Hastings-Troyer 2015)

Source note: [[Progress Towards Practical Quantum Variational Algorithms (Wecker, Hastings, Troyer 2015) — Paper Notes]]

## Primary Sources Checked

- Wecker, Hastings, and Troyer, "Progress towards practical quantum variational algorithms," Physical Review A 92, 042303 (2015) / arXiv:1507.08969.

## Verdict

Major issue in the historical phrasing; otherwise minor. The note gets the paper's negative-positive role in early VQE right, but it incorrectly says the alternating/adiabatic ansatz predates QAOA.

## Line-Anchored Comments

- Lines 17-25: the high-level story is good. Correct the historical statement: this arXiv paper appeared in 2015, after Farhi-Goldstone-Gutmann's 2014 QAOA proposal. The ansatz is QAOA-like, not predating QAOA.
- Lines 31-43: the ansatz structure is clear. For the Hubbard case, the reference is a free-fermion/hopping ground state rather than literally `|+>^n`; the parenthetical transverse-field example is fine but should not be confused with the chemistry/lattice setting.
- Lines 45-56: the circuit-depth summary is useful, but "faster than UCC" should be tied to the specific Hubbard instances and circuit-depth metric studied.
- Lines 58-68: the measurement-scaling formula needs more careful estimator language. Depending on equal versus optimized shot allocation and independent versus grouped measurement, bounds involve sums of absolute coefficients, coefficient squares, variances, and covariances differently.
- Line 70: `M ~ O(L/epsilon^2)` for Hubbard needs a precision convention. Estimating total energy to fixed absolute precision, energy density to fixed precision, and local observables are different tasks.
- Lines 78-96: the key-results table is helpful but should label the chemistry measurement counts as early/naive or conservative estimates.
- Line 109: later papers did improve the picture, but "more like `O(N^5)`" or `O(N^2.75)` should be identified as empirical/setting-dependent rather than a universal replacement for the Wecker-Hastings-Troyer bound.
- Lines 113-120: the caveats are good. The first caveat should explicitly mention basis-rotation grouping as a variance/covariance improvement, not just grouping count.

## Missing or Suggested Additions

- Add a short note distinguishing extensive Hamiltonian energy from energy density. This matters for whether a lattice-model measurement count is "tractable" as system size grows.

