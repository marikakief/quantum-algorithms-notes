# Review: High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021)

Source note: [[High-Precision Quantum Algorithms for Partial Differential Equations (Childs-Liu-Ostrander 2021) — Paper Notes]]

## Primary Sources Checked

- Childs, Liu, and Ostrander, "High-precision quantum algorithms for partial differential equations", Quantum 2021 / arXiv:2002.07868.

## Verdict

Major smoothness wording issue. The PDE summary is otherwise strong, but, as with the ODE spectral-method note, it overstates what plain `C^\infty` regularity guarantees. The polylog precision story depends on quantitative high-derivative bounds, not just smoothness as a qualitative property.

## Line-Anchored Comments

- Lines 7-17: good PDE model and global strict diagonal dominance condition.
- Lines 19-25: the main insight is right: high-precision QLSA is not enough unless discretization error also has high-precision behavior.
- Line 23: "for smooth solutions, truncated Fourier/Chebyshev series have error decaying faster than any polynomial" should be qualified. This requires quantitative regularity/derivative bounds; arbitrary `C^\infty` is not sufficient for the displayed rates.
- Lines 29-45: the adaptive finite-difference section is clear.
- Lines 51-69: the spectral method construction is well summarized.
- Line 53: again, replace "For `C^\infty` solutions" with a statement involving the paper's `g'` or derivative-bound assumptions.
- Lines 75-89: useful comparison table.
- Lines 91-97: good caveats, especially high-order derivatives, strict diagonal dominance, and output-state readout. Move the derivative caveat nearer the headline result.
- Line 126: good note that the structured PDE Hamiltonian-simulation entry may be a duplicate or overlapping entry. That should be resolved in the vault inventory later.

## Missing or Suggested Additions

- Add a small assumptions table: boundary condition, coefficient assumptions, diagonal dominance constant, derivative-bound parameter, output norm parameter, and state-preparation assumptions.

