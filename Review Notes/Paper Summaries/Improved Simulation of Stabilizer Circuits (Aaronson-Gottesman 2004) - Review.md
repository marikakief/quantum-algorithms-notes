# Review: Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004)

Source note: [[Improved Simulation of Stabilizer Circuits (Aaronson-Gottesman 2004) — Paper Notes]]

## Verdict

Minor-to-major issues. The tableau algorithm is explained well, but the complexity-class decision problem should be checked against the paper's exact formulation. Since the `oplus L` completeness statement is a central theorem, the decision problem cannot be left approximate.

## Line-Anchored Comments

- Lines 9-13: For single-qubit Pauli-basis measurements on stabilizer states, outcomes are deterministic or uniformly random. Make clear that this statement is for the specified measurement model, not arbitrary POVMs on stabilizer states.

- Lines 17-23: The `oplus L` claim is important. Verify whether the complete decision problem is "does the final measurement output 1 with nonzero probability," "with certainty," or another promise variant. The note currently says "with certainty," which may not match the paper's formal problem.

- Lines 27-50: The destabilizer tableau explanation is excellent. Keep the invariant list; it is the right way to understand why measurements become cheaper.

- Lines 52-63: The phase convention in `rowsum` should be checked against the original table. A one-line example multiplying `X` and `Z` into `Y` would prevent sign-convention errors.

- Lines 80-97: The measurement algorithm is the strongest technical section. Add that the random outcome is sampled classically and then inserted into the stabilizer update.

- Lines 109-113: The canonical form and `O(n^2/log n)` gate count apply to unitary Clifford circuits. The note says this later, but the result line should include it.

## Missing or Suggested Additions

- Add the exact formal `oplus L` problem statement from the paper.
- Add a warning that adding even a small number of non-Clifford gates changes the simulation cost exponentially in that number.
