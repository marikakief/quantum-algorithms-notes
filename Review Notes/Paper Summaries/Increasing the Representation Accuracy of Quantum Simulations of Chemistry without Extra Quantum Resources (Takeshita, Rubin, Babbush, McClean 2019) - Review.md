# Review: Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita-Rubin-Babbush-McClean 2019)

Source note: [[Increasing the Representation Accuracy of Quantum Simulations of Chemistry without Extra Quantum Resources (Takeshita, Rubin, Babbush, McClean 2019) — Paper Notes]]

## Primary Sources Checked

- Takeshita, Rubin, Jiang, Lee, Babbush, McClean, "Increasing the representation accuracy of quantum simulations of chemistry without extra quantum resources," PRX 10, 011004 (2020) / arXiv:1902.10679.

## Verdict

Minor issues. The note gives a clear explanation of VQSE and orbital relaxation and keeps the "no extra quantum resources" claim properly tied to extra measurement/classical postprocessing.

## Line-Anchored Comments

- Lines 19-25: the summary is good. Make explicit that "no extra quantum resources" does not mean no extra samples; VQSE can require high-order RDM measurements.
- Lines 39-59: the Wick contraction explanation is detailed and mostly clear. Add a sentence saying the virtual orbitals are unoccupied in the active-space reference, which is why virtual contractions are classical.
- Lines 61-68: re-check the restricted active-space scaling statement. The measurement burden depends on which active/core/virtual indices remain in the measured RDMs; `N_Av^8` by itself may not describe the intended reduction.
- Lines 71-81: orbital relaxation is well explained and correctly presented as complementary to VQSE.
- Lines 83-89: the cumulant approximation should be flagged as uncontrolled in hard strongly correlated cases, not merely "systematically improvable."
- Lines 120-124: the relationship to classical MRCI/CASSCF is exactly the right context.

## Missing or Suggested Additions

- Add a table separating VQSE, orbital relaxation, and cumulant truncation by required RDM order, quantum measurements, and classical approximation introduced.
