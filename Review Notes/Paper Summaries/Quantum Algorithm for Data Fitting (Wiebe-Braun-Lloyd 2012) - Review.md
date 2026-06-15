# Review: Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012)

Source note: [[Quantum Algorithm for Data Fitting (Wiebe-Braun-Lloyd 2012) — Paper Notes]]

## Primary Sources Checked

- Wiebe, Braun, and Lloyd, "Quantum algorithm for data fitting", PRL 2012 / arXiv:1204.5242.

## Verdict

Minor issues. The note is careful about the HHL caveats and the least-squares embedding. The main improvements are to soften the historical "opened quantum ML" claim and clarify rank-deficient versus well-conditioned pseudoinverse assumptions.

## Line-Anchored Comments

- Add a top-level title.
- Lines 7-20: good least-squares problem setup.
- Lines 24-28: the historical claim that every later linear-algebraic QML paper inherits this template is too broad. Say "many early QML linear-algebra algorithms inherited this template."
- Lines 34-42: the Hermitian embedding is clear.
- Lines 44-58: good outline of Algorithm 1. The cross-link label "Hamiltonian Simulation by Qubitization" is anachronistic for a 2012 phase-estimation method; the method being described is HHL-style phase estimation and sparse Hamiltonian simulation.
- Lines 75-89: theorem scalings are useful. Check whether `log(N)` should be `log(N+M)` or hidden in the note's `N` convention.
- Lines 93-99: the comparison table is useful but should say the quantum algorithm prepares a state or estimates fit quality; it does not output the full least-squares vector efficiently.
- Lines 114-127: excellent caveats. Keep these prominent.
- Lines 126-127: "possibly rank-deficient" in the opening should be reconciled with "assumes `F^\dagger F` invertible." If the pseudoinverse handles rank deficiency only above a singular-value cutoff, say that.

## Missing or Suggested Additions

- Add the singular-value cutoff/condition-number definition for the rectangular matrix. That is what controls whether the pseudoinverse step is well behaved.

