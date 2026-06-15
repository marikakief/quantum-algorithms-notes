# Review: Basis Switching via QFT for Kinetic-Potential Splitting

Source note: [[Basis Switching via QFT for Kinetic-Potential Splitting]]

## Primary Sources Checked

- Ortiz et al., *Quantum Algorithms for Fermionic Simulations*, arXiv:cond-mat/0012334: https://arxiv.org/abs/cond-mat/0012334
- Kassal et al., *Polynomial-time quantum algorithm for the simulation of chemical dynamics*, arXiv:0801.2986: https://arxiv.org/abs/0801.2986

## Verdict

Minor issues. The trick is correct and clearly stated. The cost line needs arithmetic/precision caveats.

## Line-Anchored Comments

- Lines 7 and 11-20: Correct: use the QFT to alternate between representations in which kinetic and potential terms are diagonal.

- Lines 20 and 30: The `O(n^2)` potential phase count is only the number of particle pairs, not the gate cost of evaluating pair potentials. For Coulomb interactions, arithmetic precision can dominate.

- Line 30: QFT cost `O(n l^2)` is the exact-QFT textbook estimate; approximate QFT can reduce constants/log factors depending on required precision.

- Lines 32-34: Good caveat. Add that the method is a product formula and therefore has timestep error controlled by commutators/nested commutators.

## Missing or Suggested Additions

- Separate "number of diagonal terms/pairs" from "arithmetic gate count to compute each phase."
- Add a convention note for whether QFT or inverse QFT maps the stored basis to momentum.
