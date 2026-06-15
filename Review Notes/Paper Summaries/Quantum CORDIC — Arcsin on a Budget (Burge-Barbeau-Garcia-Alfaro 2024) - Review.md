# Review: Quantum CORDIC - Arcsin on a Budget (Burge-Barbeau-Garcia-Alfaro 2024)

Source note: [[Quantum CORDIC — Arcsin on a Budget (Burge-Barbeau-Garcia-Alfaro 2024) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2411.14434
- Code repository linked by the note: https://github.com/iain-burge/QuantumCORDIC

## Verdict

Minor issues. The note is detailed and technically engaged. The main issue is comparison hygiene: CNOT count, Toffoli count, T count, arithmetic depth, and rotation-synthesis cost should not be mixed casually.

## Comments

- The paper's headline costs are mostly CNOT/layer/qubit costs. The comparison table should avoid ranking it directly against Toffoli- or T-count results without converting cost models.

- The identity for avoiding square roots is useful, but the fixed-point range and endpoint behavior should be stated. Errors near 0 and 1 can matter for amplitude loading.

- The "inequality-test trick is almost always better" caveat is good, but it should say "for amplitude transduction from stored nonnegative coefficients." If one actually needs the arcsine value coherently, CORDIC remains relevant.

- The note should separate computing an angle register from using CORDIC direction bits to drive rotations. These are related but not identical output formats.

- If controlled rotations are left as arbitrary rotations, their fault-tolerant synthesis cost must be included before comparing against Clifford+T arithmetic papers.

## Suggested Additions

- Add a cost-model box: qubits, CNOTs, Toffolis/T gates if known, rotation assumptions, and parallel-addition assumptions.
- Add a short note on approximation guarantees in fixed-point arithmetic.
