# Review: Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021)

Source note: [[Time-Dependent Unbounded Hamiltonian Simulation with Vector Norm Scaling (An-Fang-Liu 2021) — Paper Notes]]

## Primary Sources Checked

- An, Fang, Liu, "Time-dependent unbounded Hamiltonian simulation with vector norm scaling," Quantum 5, 459 (2021) / arXiv:2012.13105.

## Verdict

Minor issues. This is a strong note. It correctly identifies the state-dependent/vector-norm improvement and does not oversell it as a worst-case operator-norm result.

## Line-Anchored Comments

- Lines 9-21: the computational problem and motivation are clearly stated. Keep the distinction between `||H_1||` and `||H_1 psi||` front and center.
- Lines 29-39: the standard versus generalized time-dependent Trotter variants are explained well. Add a reminder that the generalized formula requires accurate classical integration of the scalar coefficients.
- Lines 45-55: the operator-norm comparison is good, but the constants `alpha_s`, `beta_s`, and `alpha_g` are paper-specific. If these are used elsewhere, link them to the precise theorem definitions.
- Lines 60-78: the vector-norm table is the heart of the note. State that the global bound follows along the exact solution trajectory; it is not an a posteriori bound using only the approximate state.
- Lines 91-101: Assumption 1 is handled carefully. The warning that it must be checked for `A` and `B` together should be kept.
- Lines 113-115: the qHOP comparison is useful, but make clear that qHOP and vector-norm scaling solve different discretization/regime problems.

## Missing or Suggested Additions

- Add one explicit smooth-wavefunction example explaining why `||(-Delta) psi||` can stay bounded as the grid is refined.
