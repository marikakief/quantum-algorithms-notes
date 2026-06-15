# Review: A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008)

Source note: [[A Quantum Algorithm to Solve Nonlinear Differential Equations (Leyton-Osborne 2008) — Paper Notes]]

## Primary Sources Checked

- Leyton and Osborne, "A quantum algorithm to solve nonlinear differential equations", arXiv:0812.4423.

## Verdict

Minor issues. The note captures the historical role and the exponential-copy bottleneck well. The main fixes are about overclaiming later progress and tightening a few model assumptions.

## Line-Anchored Comments

- Lines 21-25: the high-level assessment is good. The final sentence about a "polynomial-in-log t" algorithm being partially answered by Carleman methods should be softened. Later dissipative Carleman algorithms obtain polynomial or near-linear dependence on evolution time under strong stability assumptions, not polylogarithmic dependence on time.
- Lines 23, 100-102: the exponential dependence is really in the number of Euler steps `m=t/h`; the note says this correctly later. Prefer that phrasing over "exponential in `t` and inverse step size" because `t` and `1/h` are not independent in the actual resource expression.
- Lines 45 and 106: good to state the two sparsity promises separately. Consider adding that these are promises on the polynomial description and variable incidence graph, not merely on the vector state.
- Lines 51-57: the use of `epsilon` for both short simulation time and target accuracy is potentially confusing. This paper's small Hamiltonian-evolution parameter is not the final ODE accuracy parameter used in later algorithm comparisons.
- Lines 61-63: the displayed runtime and error bounds are useful, but they introduce `kappa`, `gamma`, and `eta` without enough local definition. A reader cannot check the scaling without knowing which Hamiltonian-simulation bound and sparsity parameters are being invoked.
- Lines 89 and 120: citing HHL as context is historically fine because HHL circulated as arXiv:0811.3171 before this preprint. Avoid presenting HHL as an established prior literature line in 2008 without that arXiv-date caveat.
- Line 108: "the analysis is correct" is stronger than needed for a review note. Better: "the analysis is internally consistent for the stated model but loose and limited."

## Missing or Suggested Additions

- Add one sentence separating "nonlinear map on amplitudes" from "a physical nonlinear quantum operation". The algorithm remains linear quantum mechanics plus postselection on tensor-product copies.
- When comparing to Carleman methods, state the stability regime (`R<1` or log-norm/dissipativity variants) so the comparison is not read as a general nonlinear ODE solver.
