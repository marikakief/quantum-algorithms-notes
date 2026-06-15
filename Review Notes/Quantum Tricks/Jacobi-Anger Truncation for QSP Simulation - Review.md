# Review: Jacobi-Anger Truncation for QSP Simulation

Source note: [[Jacobi-Anger Truncation for QSP Simulation]]

## Primary Sources Checked

- Low and Chuang, "Optimal Hamiltonian Simulation by Quantum Signal Processing," arXiv:1606.02685: https://arxiv.org/abs/1606.02685

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 8-12: good high-level explanation. This card correctly places approximation theory at the center of QSP simulation.
- Lines 16-20: the sparse-Hamiltonian cost expression is appropriate for the Low-Chuang sparse model. Make clear that `tau = td||H||max` is the normalized time parameter.
- Line 18: depending on which QSP/qubitization theorem is being quoted, the precision term may be written with source-specific shorthand. For consistency with the rest of the vault, prefer `log(1/epsilon)/loglog(1/epsilon)` unless explicitly quoting the PRL abstract's normalized `O(t + log(1/epsilon))` language.

## Missing or Suggested Additions

- Add one formula for the Jacobi-Anger expansion or Bessel-tail bound. The current card tells the reader what happens but not the identity that makes it happen.
