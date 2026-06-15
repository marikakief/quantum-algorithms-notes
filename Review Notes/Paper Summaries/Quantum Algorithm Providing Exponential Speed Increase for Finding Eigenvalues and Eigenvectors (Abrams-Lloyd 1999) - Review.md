# Review: Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999)

Source note: [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]]

## Verdict

Minor issues. The note gives a historically sensible account of phase-estimation-based eigenvalue finding. It should keep the exponential-speedup claim tied to representation, overlap, and Hamiltonian-simulation assumptions.

## Comments

- The algorithm succeeds on an eigenstate with probability equal to the input state's squared overlap with that eigenstate. This should be in every headline complexity statement, not only the step-by-step derivation.

- Eigenvalues are recovered through phases of `e^{-iHt}` and are therefore modulo the chosen evolution time. Precision, aliasing, and energy-range assumptions should be stated together.

- The historical `O(t^2/epsilon)` simulation cost is useful as original-paper context, but the note should not use it as a modern complexity benchmark.

- The "50-100 qubits" discussion is valuable historically. It should be labeled as an early estimate with pre-threshold, pre-resource-estimation assumptions.

- The note correctly distinguishes the heap-sort antisymmetrization issue from this paper. Keep that distinction; it prevents a common misattribution.

## Suggested Additions

- Add a compact comparison between Kitaev eigenvalue measurement, textbook inverse-QFT phase estimation, and Abrams-Lloyd's presentation.
- State explicitly that the output is a sampled eigenpair, not a full spectrum or a classical eigenvector description.
