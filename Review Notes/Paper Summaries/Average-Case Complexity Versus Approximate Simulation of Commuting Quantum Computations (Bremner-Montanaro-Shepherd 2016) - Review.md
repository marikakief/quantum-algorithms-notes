# Review: Average-Case Complexity Versus Approximate Simulation of Commuting Quantum Computations (Bremner-Montanaro-Shepherd 2016)

Source note: [[Average-Case Complexity Versus Approximate Simulation of Commuting Quantum Computations (Bremner-Montanaro-Shepherd 2016) — Paper Notes]]

## Primary Sources Checked

- Bremner, Montanaro, and Shepherd, "Average-case complexity versus approximate simulation of commuting quantum computations," arXiv:1504.07999; Phys. Rev. Lett. 117, 080501 (2016).

## Verdict

Minor issues. The note is accurate and clearly separates proven anticoncentration from conjectured average-case hardness. That separation is the central intellectual structure of the paper.

## Line-Anchored Comments

- Lines 9-19: good framing. The additive-error upgrade over Bremner-Jozsa-Shepherd is correctly stated as conditional.
- Lines 29-36: Stockmeyer-to-probability-estimation pipeline is clear. Note that Stockmeyer gives an `FBPP^NP`-type estimator for the sampler's probabilities, not direct access to the true quantum probabilities without the total-variation closeness argument.
- Lines 40-48: good statement of the two average-case conjectures. Keep the `1/24` fraction and multiplicative `1/4+o(1)` constants if precision matters.
- Lines 50-67: excellent anticoncentration explanation. This is the proven part that distinguishes IQP from the original BosonSampling conjecture package.
- Lines 70-80: theorem table is useful. Be careful that the Ising fourth-moment scaling line uses the unnormalized partition function, while amplitude moments use `2^{-n}` scaling.
- Lines 97-104: nonlocal-gate implementation caveat is important. The theorem is complexity-theoretic, not a direct nearest-neighbor hardware proposal.
- Lines 108-113: limitations are complete. The `1/192` total-variation threshold is stringent and is not a generic noise-tolerance guarantee.

## Missing or Suggested Additions

- Add a diagram of the proof dependencies: approximate sampler -> Stockmeyer -> multiplicative estimator on many outputs -> average-case #P conjecture -> PH collapse.
- Add a note that proven anticoncentration does not by itself imply hardness; the average-case conjecture remains necessary.

