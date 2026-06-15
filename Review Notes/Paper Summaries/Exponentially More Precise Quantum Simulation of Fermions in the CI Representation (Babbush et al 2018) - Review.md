# Review: Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018)

Source note: [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]]

## Primary Sources Checked

- Babbush et al., "Exponentially More Precise Quantum Simulation of Fermions in the Configuration Interaction Representation", arXiv:1506.01029 / QST 3, 015006 (2018).
- Compared against the later first-quantized plane-wave line represented by Babbush et al. 2019 and Su et al. 2021.

## Verdict

Major issues. The high-level story is right, but the note repeatedly overstates the qubit advantage as "exponential", contains a false asymptotic comparison condition, and has a numerical example off by a factor of ten.

## Line-Anchored Comments

- Lines 21 and 33: "exponentially better" / "exponentially fewer qubits" is not the right statement. The CI encoding uses `eta log N` qubits versus `N` occupation-number qubits, so it can be dramatically smaller when `eta log N << N`; this is not an exponential separation in the usual input parameters. The title's "exponentially more precise" refers to precision dependence, not qubit count.
- Line 21: the gate-count improvement "when `eta << N`" needs the stronger condition `eta^2 << N` if comparing `eta^2 N^3` to `N^4`. The note repeats the weaker condition at line 98.
- Lines 47-51: the discretization and rounding discussion is reasonable but hides the role of the orbital regularity assumptions in Theorem 1. This should be presented as a theorem under specific assumptions, not as a generic CI-matrix oracle construction.
- Line 77: `r = O(eta^2 N^2 ||lambda|| t)` is not a clean way to state the segmentation count. The source's final theorem is the safer object: after bounding the relevant coefficient norm and select cost, the total gate complexity is `~O(eta^2 N^3 t)`.
- Lines 93-96: the comparison table mixes several cost models: Trotter gate counts, Taylor-series database costs, and on-the-fly oracle costs. Add a note that these are source-specific asymptotic comparisons, not uniform compiled Toffoli estimates.
- Line 98: "`eta << N` implies `eta^2 N^3 << N^4`" is false. It requires `eta << sqrt(N)`.
- Line 112: the O'Malley/NJP connection is muddled. NJP 18, 033032 is the second-quantized Taylor-series paper, not this CI-representation QST paper. Say the experiment cites Taylor-series chemistry methods from the same program; do not identify that citation with an early version of the CI paper without checking the exact reference.
- Line 114: for `N = 100`, `eta = 10`, the leading monomial `eta^2 N^3` is `10^8`, not `10^7`.
- Line 118: "plane-wave bases generally do" is doubtful in this context. The CI paper's assumptions are about efficiently evaluable, localized orbital basis functions with bounded derivatives and decay behavior. Plane waves are delocalized and belong to the separate first-quantized plane-wave algorithmic line.

## Missing or Suggested Additions

- Add a one-sentence distinction between first-quantized CI in an orbital determinant basis and later first-quantized plane-wave particle-register algorithms. They share fixed-particle encoding but not the same Hamiltonian oracle.
- Replace "exponentially fewer qubits" with "asymptotically fewer qubits when the electron count is much smaller than the one-particle basis size."

