# Review: Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019)

Source note: [[Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs (Mann-Bremner 2019) — Paper Notes]]

## Primary Sources Checked

- Mann and Bremner, "Approximation Algorithms for Complex-Valued Ising Models on Bounded Degree Graphs," Quantum 3, 162 (2019); arXiv:1806.11282.

## Verdict

Minor issues. The note is accurate and well scoped. It correctly presents this as a classical easy regime for complex Ising/IQP amplitudes, not a contradiction of IQP hardness results.

## Line-Anchored Comments

- Lines 9-15: good problem statement. The bounded-degree condition and small-complex-parameter condition are both essential.
- Lines 21-25: high-level summary is accurate. "Small enough" should always be read through the zero-free polyregion, not through a generic perturbative phrase.
- Lines 31-45: Barvinok/Patel-Regts pipeline is clear.
- Lines 51-57: key corollaries are stated well. The IQP amplitude formula is exactly the useful bridge to the quantum-sampling literature.
- Lines 61-72: comparison table is good. It would be useful to distinguish FPRAS (randomized) from deterministic FPTAS consistently.
- Lines 75-83: caveats are complete. The small-angle IQP implication is especially important because otherwise readers may think this classically simulates the hard IQP families.

## Missing or Suggested Additions

- Add an "easy side of the phase diagram" callout: bounded degree plus small complex couplings is a zero-free/high-temperature-like regime.
- Cross-link directly to the BMS 2016 average-case IQP note as the complementary hard regime.

