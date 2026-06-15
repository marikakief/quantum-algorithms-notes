# Review: Quantum Algorithms for Systems of Linear Equations Inspired by Adiabatic Quantum Computing (Subasi-Somma-Orsucci 2018)

Source note: [[Quantum Algorithms for Systems of Linear Equations Inspired by Adiabatic Quantum Computing (Subaşı-Somma-Orsucci 2018) — Paper Notes]]

## Verdict

Minor issues. The summary captures the adiabatic-inspired QLSA idea and its relation to HHL well. The main missing pieces are access-model and output-model caveats.

## Comments

- The headline complexity should specify the matrix-access model: sparse Hamiltonian simulation or block-encoding access, plus efficient preparation of `|b>`.

- The algorithm returns a quantum state proportional to the solution. As with HHL, the cost of extracting classical information can dominate unless only a few observables are needed.

- The dependence on `epsilon` should be stated carefully. Later QLSA papers improve different parameter regimes; do not compare them without matching the error norm and oracle model.

- If the note describes the method as "adiabatic quantum computing," clarify that the final algorithm is implemented through a discretized/evolved path and analyzed in the circuit/query model.

- The non-Hermitian extension should mention the usual Hermitian embedding and how that affects condition number and normalization.

## Suggested Additions

- Add a comparison row with HHL, Childs-Kothari-Somma, and Costa et al. only after aligning `kappa`, sparsity/block-encoding normalization, and precision conventions.
