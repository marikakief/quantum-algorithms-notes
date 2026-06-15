# Review: Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020)

Source note: [[Predicting Many Properties from Very Few Measurements (Huang-Kueng-Preskill 2020) — Paper Notes]]

## Verdict

Minor issues. This is a strong summary of classical shadows, with the right emphasis on the shadow norm and median-of-means. The fixes are mostly precision: a corrupted TeX character, one author-name slip, and a few places where local-Pauli and general-Clifford guarantees should be separated.

## Line-Anchored Comments

- Line 23: "Robert Huang" should be "Hsin-Yuan Huang."

- Lines 27-41: There is a corrupted control character in the displayed estimator, visible around the intended `\!\bigl(...)`. Replace it with `\!\bigl(...)` or simply `\mathcal{M}^{-1}(U^\dagger |\hat b\rangle\langle \hat b| U)`.

- Lines 43-52: Good explicit inverses. Add that global Clifford shadows and local Clifford/Pauli shadows are different ensembles with different implementation and post-processing costs.

- Lines 78-90: The theorem statement is useful. The trace-subtracted observable should remain in the bound; otherwise identity components make the norm discussion look mysterious.

- Lines 94-112: The local bound is correct at the level of a general `k`-local observable, but for Pauli strings the familiar factor is `3^k` rather than `4^k`. A one-line note would prevent confusion with Pauli measurement schedules.

- Lines 114-128: The lower-bound discussion should specify the measurement model: fixed single-copy measurements. It does not rule out collective, adaptive, or structure-specific schemes.

- Lines 130-136: The nonlinear-functions paragraph is good but should emphasize that estimating nonlinear quantities requires independent shadow copies and usually worsens constants/sample complexity.

## Missing or Suggested Additions

- Add a quick "what is not a density matrix" warning near the snapshot definition.
- Add a small example comparing local Pauli shadows for local Hamiltonian terms with global Clifford shadows for fidelity estimation.
