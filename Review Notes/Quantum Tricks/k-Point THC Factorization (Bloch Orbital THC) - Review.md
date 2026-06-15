# Review: k-Point THC Factorization (Bloch Orbital THC)

Source note: [[k-Point THC Factorization (Bloch Orbital THC)]]

## Primary Sources Checked

- Rubin et al., arXiv:2302.05531 / PRX Quantum 4, 040303 (2023).

## Verdict

Minor issues. The trick card is basically right, but benchmark-dependent rank claims should be marked as empirical.

## Line-Anchored Comments

- Line 6: the statement that the central tensor depends on momentum transfer data rather than four independent momenta is the right conceptual simplification.
- Line 28: `c_THC = 8` and `M = 4N` with small MP2 errors is a benchmark observation, not a universal accuracy theorem for all periodic materials.
- Line 41: the block-encoding statement is useful, but "no symmetry speedup" should be interpreted for the second-quantized Bloch-orbital construction being discussed, not every possible periodic-materials algorithm.

## Missing or Suggested Additions

- Add one sentence saying that k-point THC saves classical data/storage structure and can reduce constants, while the quantum resource outcome still depends on `lambda` and compiled PREPARE/SELECT costs.

