# Review: Low-Depth Quantum Simulation of Materials (Babbush-Wiebe-McClean et al. 2018)

Source note: [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]]

## Primary Sources Checked

- Babbush, Wiebe, McClean, McClain, Neven, Chan, "Low-Depth Quantum Simulation of Materials," PRX 8, 011044 (2018) / arXiv:1706.00023.

## Verdict

Minor issues. The note is strong and captures why the plane-wave dual basis was a practical turning point for materials simulation.

## Line-Anchored Comments

- Lines 11-13: the high-level summary is good. The phrase "fixed absolute precision (bounded in system size)" is confusing because the displayed measurement cost later scales as `O(N^4/epsilon^2)`.
- Lines 25-33: the diagonal potential/diagonal kinetic split is well explained. Add a clearer note that the dual basis is most natural for periodic boundary conditions.
- Lines 53-58: the `O(N)` depth for the `O(N^2)` diagonal interaction layer depends on the planar scheduling construction. The caveat appears later; it should also be visible here.
- Lines 60-72: the Trotter-depth scaling is very useful, but the simplification to fixed charge density should spell out which variables are being identified or held constant.
- Lines 81-90: the Taylor/LCU comparison should keep `lambda`, gate count, and depth separate. The table mixes them compactly.
- Lines 111-127: the jellium NISQ proposal is described fairly. Keep the caveat that this is a variational upper bound, not a certified ground-state algorithm.

## Missing or Suggested Additions

- Add a one-sentence contrast with later first-quantized plane-wave algorithms: this paper optimizes second-quantized depth for periodic systems, while later first-quantized work optimizes asymptotic basis-size scaling.
