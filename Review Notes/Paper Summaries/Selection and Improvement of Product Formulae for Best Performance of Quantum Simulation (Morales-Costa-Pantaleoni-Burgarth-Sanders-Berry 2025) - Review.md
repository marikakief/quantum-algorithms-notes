# Review: Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025)

Source note: [[Selection and Improvement of Product Formulae for Best Performance of Quantum Simulation (Morales-Costa-Pantaleoni-Burgarth-Sanders-Berry 2025) — Paper Notes]]

## Primary Sources Checked

- Morales, Costa, Pantaleoni, Burgarth, Sanders, Berry, "Selection and improvement of product formulae for best performance of quantum simulation," arXiv:2210.15817: https://arxiv.org/abs/2210.15817

## Verdict

Major issues only in rhetoric. The technical summary is useful, but the note overstates several practical conclusions as universal facts.

## Line-Anchored Comments

- Line 17: "8th order is optimal" should be scoped to the parameter ranges, cost model, formula families, and examples studied. It is not a theorem that 8th order is always optimal for quantum simulation.
- Line 19: the personal relevance sentence should be moved out of the paper-summary body unless the vault intentionally includes personal annotations.
- Lines 68-73: the figure of merit `M zeta^{1/k}` is well explained and should stay.
- Lines 82-87: "10th order ... irrelevant for quantum computing" is too strong. Say the paper argues 10th order is unnecessary in the tested/relevant chemistry parameter ranges.
- Lines 129-137: the caveats are good, especially the warning that norm-based selection can be misleading.
- Line 135: the randomization implication is speculative. It is a useful hypothesis, but it should not be presented as a settled conclusion about qDRIFT/randomized formulas generally.

## Missing or Suggested Additions

- Add the exact meaning of eigenvalue error used in the paper before using it as the comparison metric.
- State which Hamiltonian classes were tested numerically and which conclusions are extrapolations.
