# Review: First-Quantized Plane-Wave Chemistry Encoding

Source note: [[First-Quantized Plane-Wave Chemistry Encoding]]

## Primary Sources Checked

- Babbush, Berry, McClean, and Neven, *Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size*, arXiv:1807.09802, npj Quantum Information 5, 92 (2019): https://arxiv.org/abs/1807.09802
- Berry et al., *Improved techniques for preparing eigenstates of fermionic Hamiltonians*, npj Quantum Information 4, 22 (2018): https://www.nature.com/articles/s41534-018-0071-5

## Verdict

Major issues. The encoding description is mostly right, but the card makes a wrong qubit-count comparison: the `(N/eta)^(2/3)` advantage is not the ratio of qubit counts. It belongs to algorithmic scaling comparisons in a particular plane-wave simulation regime, not memory savings.

## Line-Anchored Comments

- Lines 7 and 13-15: Correct: first quantization uses one register per electron, each storing an orbital/grid momentum index, for `O(eta log N)` qubits.

- Lines 17-25: Good explanation that antisymmetry lives in amplitudes rather than JW strings. Add that state preparation must ensure both antisymmetry and no duplicate electron labels when appropriate.

- Line 23: The sorting-network antisymmetrization reference is good. The exact gate complexity depends on the chosen sorting network, comparator arithmetic, and precision/model; keep the big-O scoped.

- Line 31: The potential 1-norm and kinetic norm scalings need their assumptions: fixed density/volume conventions, charge normalization, and plane-wave grid geometry. Without those, the line looks universal.

- Line 35: This is the main error. The qubit-count advantage over second quantization is roughly `N` versus `eta log N` qubits, not `(N/eta)^(2/3)`. The `(N/eta)` exponents arise in gate-complexity comparisons for particular algorithms, not the storage ratio.

- Line 37: The statement that `N approx 100 eta` is typical for plane-wave chemical accuracy is plausible as a rule of thumb, but it should be source-labeled and not treated as universal across molecules, pseudopotentials, and basis choices.

- Line 48: Good caveat: explicit particle registers mean SELECT/oracle work often carries factors of `eta`.

- Line 50: The basis-error discussion is useful but should be stated cautiously. Plane waves and Gaussians have different convergence behavior depending on cusps, pseudopotentials, periodicity, and boundary conditions.

## Missing or Suggested Additions

- Replace the qubit-count comparison with `N / (eta log N)` up to constants.
- Add a separate "why sublinear in basis size" paragraph for the interaction-picture algorithmic scaling.
- Mention that antisymmetric subspace preservation requires the simulated Hamiltonian to commute with particle permutations.
