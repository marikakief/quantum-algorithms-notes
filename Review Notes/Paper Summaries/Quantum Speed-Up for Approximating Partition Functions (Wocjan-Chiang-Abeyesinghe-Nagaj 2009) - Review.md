# Review: Quantum Speed-Up for Approximating Partition Functions (Wocjan-Chiang-Abeyesinghe-Nagaj 2009)

Source note: [[Quantum Speed-Up for Approximating Partition Functions (Wocjan-Chiang-Abeyesinghe-Nagaj 2009) — Paper Notes]]

## Verdict

Minor issues. The note captures the two-part speedup story well: coherent Markov-chain walks and amplitude estimation. The main fix is to make the Markov-chain and cooling-schedule assumptions unavoidable.

## Comments

- The result depends on having a rapidly mixing classical Markov chain and a coherent implementation of its transition structure. The speedup is not a generic partition-function algorithm.

- The quadratic improvement in sample complexity and the quadratic improvement in annealing schedule length are coupled in this construction. Avoid phrasing that makes either improvement sound universally available on its own.

- The problem is classical partition functions/Gibbs distributions in the Markov-chain framework, not arbitrary quantum partition functions.

- The note should mention warm starts or slowly varying inverse-temperature schedules when describing the telescoping product estimator.

- If the note quotes a total complexity, include dependence on spectral gaps, minimum overlaps/fidelities between consecutive Gibbs states, and target relative error.

## Suggested Additions

- Add a comparison with classical simulated annealing that lists exactly which classical cost term is quadratically improved.
- Add a line explaining why Szegedy quantization gives phase gaps related to the classical spectral gap.
