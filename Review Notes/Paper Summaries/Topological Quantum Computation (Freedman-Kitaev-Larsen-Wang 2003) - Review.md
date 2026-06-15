# Review: Topological Quantum Computation (Freedman-Kitaev-Larsen-Wang 2003)

Source note: [[Topological Quantum Computation (Freedman-Kitaev-Larsen-Wang 2003) — Paper Notes]]

## Verdict

Major issues. The note gives the right big picture, but the level/root conventions and non-universal exceptional cases are muddled. It also overstates physical error protection by contrasting it too sharply with threshold fault tolerance.

## Line-Anchored Comments

- Lines 17-25: The paper is a survey/announcement. Keep that in the headline so readers know where the density proofs actually live.

- Lines 29-36: Check the level/root convention. The note uses "level `k=3`" and "`CS_5`" language together; that can be correct under one convention, but the labels, root of unity, and number of particle types must be made consistent.

- Lines 37-43: The qubit encoding description is useful but too terse. Add the fusion-tree constraints and leakage subspace; topological encodings are not ordinary tensor-product qubits.

- Lines 64-66: The statement "density holds for `k >= 3`, `k != 4`" needs convention clarification. The note later says the `k=4` case is abelian, which is not generally accurate under standard SU(2)_k terminology. Ising anyons are the common non-universal example, but they are non-abelian and conventionally associated with SU(2)_2.

- Lines 68-76: The topological error-protection discussion is too optimistic. Braiding gates are insensitive to smooth path deformations, but thermal anyon creation, quasiparticle poisoning, leakage, measurement errors, and finite gaps still require an error model and often active fault tolerance.

- Lines 78-84: "Polynomially equivalent" is broadly right when combining universality and TQFT simulation results, but state the direction and assumptions: approximating TQFT amplitudes is in BQP; certain anyon models can simulate BQP by dense braiding.

## Missing or Suggested Additions

- Add a convention box for `k`, `r`, root of unity, and Jones parameter.
- Add a short table of universal versus non-universal anyon models, including Fibonacci and Ising.
