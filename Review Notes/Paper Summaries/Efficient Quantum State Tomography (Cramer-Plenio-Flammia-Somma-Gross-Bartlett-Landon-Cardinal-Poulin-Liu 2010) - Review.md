# Review: Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010)

Source note: [[Efficient Quantum State Tomography (Cramer-Plenio-Flammia-Somma-Gross-Bartlett-Landon-Cardinal-Poulin-Liu 2010) — Paper Notes]]

## Verdict

Minor-to-major issues. The note captures the two reconstruction/certification schemes and the parent-Hamiltonian witness well, but it sometimes turns "certified if a good MPS candidate is found" into a stronger "assumption-free efficient tomography" statement. The main fixes are to separate measurement setting, reconstruction heuristic, and certification theorem.

## Line-Anchored Comments

- Lines 9-17: "Without any a priori assumptions" is too strong if read as a reconstruction guarantee. The certification does not assume the true state is an MPS, but the efficient reconstruction strategy is useful only when a low-bond-dimension MPS is actually found.

- Lines 15-23: "Only O(N) local measurements" should be "O(N) local reduced-density-matrix settings/block tomographies." The copy complexity still depends on local Hilbert-space dimension, block size, accuracy, and confidence.

- Lines 27-49: The sequential-disentanglement scheme is clearly explained. Add that it is a control-heavy scheme: it requires applying the learned local unitaries to fresh copies, not just passive local measurements.

- Lines 51-63: The MPS-SVT/DMRG discussion should not call the classical post-processing polynomial without qualification. DMRG is efficient in practice for the intended MPS regime, but global convergence of the modified SVT procedure is not established.

- Lines 65-75: The parent-Hamiltonian certificate is the strongest part of the note. State explicitly that the bound is only useful when the reconstructed parent Hamiltonian has a non-negligible gap; otherwise the witness may certify almost nothing.

- Lines 77-88: The table should expose dependence on `D`, `d`, `k`, local tomography precision, and the spectral gap. These are not harmless constants for a review aimed at algorithms readers.

- Lines 90-100: The comparison with shadow tomography is useful but should mark it as later context, not prior work relative to the 2010 paper.

## Missing or Suggested Additions

- Add a three-column distinction: data acquisition, MPS reconstruction, fidelity certification.
- Add a caveat that PEPS/2D generalizations are limited by contraction and parent-Hamiltonian gap issues.
- Include the exact block-size assumptions under which the local marginals determine the MPS.
