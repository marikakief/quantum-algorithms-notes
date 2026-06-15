# Review: A Variational Quantum Algorithm for Preparing Quantum Gibbs States (Chowdhury-Low-Wiebe 2020)

Source note: [[A Variational Quantum Algorithm for Preparing Quantum Gibbs States (Chowdhury-Low-Wiebe 2020) — Paper Notes]]

## Primary Sources Checked

- Chowdhury, Low, and Wiebe, "A Variational Quantum Algorithm for Preparing Quantum Gibbs States," arXiv:2002.00055 (2020).

## Verdict

Minor-to-major issues. The free-energy framing and Fourier-log idea are right, but the note makes the entropy-estimation subroutine sound much more NISQ-simple than it is. The paper's estimator uses density matrix exponentiation, amplitude/phase-estimation-style primitives, and has explicit dependence on a lower eigenvalue cutoff.

## Line-Anchored Comments

- Lines 15, 27-29, and 132-133: "near-term," "shallow circuit," and "NISQ-friendly" need qualification. The algorithm is variational in spirit, but the entropy estimator uses nontrivial quantum subroutines rather than the kind of shallow Pauli-measurement loop used in ordinary VQE.
- Lines 27 and 57-69: the Fourier approximation is described at the right conceptual level, but the operational primitive is more specific: estimate traces such as `Tr(rho cos(rho t + theta))`, using density matrix exponentiation of `rho` from purification copies. It is not just measuring simple ansatz observables.
- Lines 57-63: the logarithm approximation needs a spectral interval lower bound `p_min > 0`. The singularity of `log x` at zero is a central reason the cost depends badly on small eigenvalues of `rho`.
- Lines 69-75: "powers or controlled functions of the ansatz state" should be replaced with density-matrix exponentiation plus iterative phase-estimation/amplitude-estimation style measurements.
- Lines 100 and 118-120: the high-temperature/local-convergence statement is well phrased. Keep the "close enough to a global optimum" qualifier; it is essential.
- Lines 114-116: add the sample/query complexity of entropy estimation, at least qualitatively: it scales with the precision and with `p_min`, not merely with the number of Fourier terms.
- Lines 137-143: the caveats are good but should explicitly include purification-copy overhead and the assumption that the ansatz can prepare a state with controlled minimum eigenvalue.
- Lines 147-149: the trick-card names are useful. The entropy-estimation card should not be presented as a generic shallow measurement trick; it is a Fourier plus density-matrix-exponentiation trick.

## Missing or Suggested Additions

- Add a resource-model paragraph separating the variational ansatz circuit from the entropy-estimation oracle model. This would prevent readers from mistaking the paper for a lightweight VQE-style protocol.

