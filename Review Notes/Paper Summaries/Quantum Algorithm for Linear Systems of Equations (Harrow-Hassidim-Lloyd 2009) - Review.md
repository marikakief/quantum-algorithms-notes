# Review: Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009)

Source note: [[Quantum Algorithm for Linear Systems of Equations (Harrow-Hassidim-Lloyd 2009) — Paper Notes]]

## Primary Sources Checked

- Harrow, Hassidim, and Lloyd, "Quantum algorithm for linear systems of equations", PRL 2009 / arXiv:0811.3171.

## Verdict

Major issues. The algorithmic summary is mostly good, but the note overstates several complexity-theoretic caveats and contradicts itself about precision dependence. HHL's `1/epsilon` state-preparation dependence was later improved; the PP caveat applies to stronger expectation-value/output-estimation tasks, not to simply preparing `|x>`.

## Line-Anchored Comments

- Add a top-level title.
- Lines 7-11: good headline, but the classical comparison is too compressed. The `O(Ns sqrt(kappa) log(1/epsilon))` comparison is a conjugate-gradient style benchmark for suitable positive-definite systems, not a universal classical baseline for all sparse linear systems.
- Lines 13 and 126-131: the BQP-completeness discussion needs careful scope. The reduction shows linear-system state preparation is BQP-hard/complete under appropriate promises. The statements about sublinear `kappa` and `polylog(1/epsilon)` need exact task definitions and should not be stated as blanket consequences.
- Lines 63-78: the phase-estimation, conditional-rotation, and postselection story is accurate.
- Lines 100-109: good breakdown of the original HHL scaling. This conflicts with line 169, which says HHL's runtime grows linearly in `kappa`; original HHL has quadratic `kappa` dependence in the summary's own table.
- Lines 128-131: "Improving epsilon dependence to polylog would give BQP contains PP" is misleading as written. Childs-Kothari-Somma later achieved polylogarithmic precision dependence for QLSP state preparation. The hardness statement should be restricted to estimating expectation values/additive output quantities, not state-preparation accuracy.
- Lines 153-157: this table is useful and correctly shows later polylog-precision QLSP algorithms.
- Lines 161-173: Aaronson's caveats are exactly the right caveats, but line 169 should be changed from "HHL's runtime grows linearly in kappa" to "at least linearly, and quadratically in the original HHL algorithm."

## Missing or Suggested Additions

- Separate three tasks: prepare the normalized solution state, estimate an observable of that state, and output the full classical solution vector. The complexity-theoretic caveats differ across these tasks.

