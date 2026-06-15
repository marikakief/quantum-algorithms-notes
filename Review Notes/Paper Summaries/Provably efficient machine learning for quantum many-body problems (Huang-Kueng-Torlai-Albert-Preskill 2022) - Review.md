# Review: Provably Efficient Machine Learning for Quantum Many-Body Problems (Huang-Kueng-Torlai-Albert-Preskill 2022)

Source note: [[Provably efficient machine learning for quantum many-body problems (Huang-Kueng-Torlai-Albert-Preskill 2022) — Paper Notes]]

## Primary Sources Checked

- arXiv metadata: https://arxiv.org/abs/2106.12627
- Science DOI metadata via NIST entry: https://www.nist.gov/publications/provably-efficient-machine-learning-quantum-many-body-problems

## Verdict

Minor issues. This is a careful and appropriately skeptical note. It correctly stresses average-case prediction, gapped phases, and poor precision dependence. The main revision is to make the theorem parameters and training-data model even more explicit.

## Line-Anchored Comments

- Lines 11-37: The problem setup is clear. Add whether the Hamiltonian family is given explicitly, sampled experimentally, or accessed through prepared ground-state shadows; the theorem's meaning changes with the source of training data.

- Lines 45-55: Good assessment. Keep "average-case, phase-restricted interpolation theorem" near the top; that phrase prevents the common overreading of this paper.

- Lines 59-71: The "one shadow snapshot per training point" statement is conceptually important, but specify that this is for the formal expectation-value guarantee and that variance/observable norm budgets still enter the theorem.

- Lines 73-129: The Fourier-interpolation explanation is good. Add that the smoothness bound comes from quasi-adiabatic continuation plus Lieb-Robinson locality, not from generic smooth parameter dependence.

- Lines 157-188: Theorem 3 is the right detailed statement. The phrase "polynomial in m" should always be paired with "for fixed `B` and constant `epsilon`"; otherwise the `m^{O(B^2/epsilon)}` term is easy to miss.

- Lines 208-238: The lower-bound theorem is a useful inclusion. Make clear that it applies to the broad smooth-family setting, not every physically local structured family one might care about.

## Missing or Suggested Additions

- Add a small "not worst-case Hamiltonian solving" warning.
- Include a one-line contrast with classical tensor-network interpolation methods, since readers may otherwise mistake the result for an algorithm that bypasses many-body complexity.
