# Review: QET-U - Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022)

Source note: [[QET-U — Ground-State Preparation and Energy Estimation on Early Fault-Tolerant QC (Dong-Lin-Tong 2022) — Paper Notes]]

## Primary Sources Checked

- Dong, Lin, and Tong, "Ground-State Preparation and Energy Estimation on Early Fault-Tolerant Quantum Computers via Quantum Eigenvalue Transformation of Unitary Matrices," PRX Quantum 3, 040305 (2022), arXiv:2204.05955.

## Verdict

Minor-to-major issues. The technical summary is strong, especially the treatment of fuzzy bisection and recursive amplitude estimation, but the opening misidentifies Lin-Tong 2022's input model and overstates the generic "no multi-qubit control" claim. Fixing those two points would make the rest of the note much more reliable.

## Line-Anchored Comments

- Line 9: Lin-Tong 2022 does not use a block-encoding model for its main ACDF energy-estimation algorithm. It uses controlled Hamiltonian evolution/Hadamard-test sampling plus classical CDF inversion. The block-encoding comparison belongs to Lin-Tong 2020.
- Line 9: "one ancilla qubit, and no multi-qubit control operations" is too broad for QET-U as a framework. The generic circuit still uses controlled `U` and `U^\dagger`; the control-free/no-MQC implementation is a structural special case discussed later in the note.
- Lines 21-27: this circuit description correctly reveals the generic controlled-evolution requirement. The opening should be reconciled with this paragraph so a reader does not think QET-U always avoids controlled time evolution.
- Lines 33-37: the fuzzy-bisection explanation is good. Add that the threshold test has a promise/fuzziness window; it does not decide exact equality near the boundary.
- Lines 41-45: the recursive QET-U amplitude-estimation description is one of the best parts of the note. It should specify whether `gamma` is an amplitude or whether the paper's overlap parameter is being converted from a probability convention.
- Lines 55: "more than enough for early fault-tolerant applications" is too sweeping. Degree 10000 is impressive for phase synthesis experiments, but whether it is enough depends on gap, precision, overlap, and Hamiltonian simulation cost.
- Lines 57-65: the control-free construction is well stated. This should be promoted into the opening caveat: no direct controlled Hamiltonian evolution is obtained only for Hamiltonians decomposable into suitable anti-commuting groups.
- Lines 87-101: the comparison table is useful but should define the overlap convention before comparing to Lin-Tong 2022, which uses a probability lower bound `eta`. Translating between `eta` and an amplitude `gamma` changes exponents by a factor of two.
- Lines 103-107: same input-model issue as line 9. Lin-Tong 2022 established the ACDF/Hadamard-test approach in a Hamiltonian-evolution model, not a block-encoding approach.
- Lines 111-116: the caveats are strong and mostly correct. They should be integrated into the first paragraph rather than appearing only after the resource tables.

## Missing or Suggested Additions

- Add a compact table separating three resource models: block encoding (Lin-Tong 2020), controlled Hamiltonian evolution plus sampling (Lin-Tong 2022 ACDF), and unitary eigenvalue transformation/QET-U (Dong-Lin-Tong 2022).
- Add a note that "no MQC" means no high-level multi-qubit controlled operations in the stated implementation model, not necessarily zero controlled gates after compiling the Hamiltonian simulation circuit.

