# Review: QSVT and Beyond (Gilyen et al. 2018-2019)

Source note: [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]

## Primary Sources Checked

- Gilyen, Su, Low, Wiebe, "Quantum singular value transformation and beyond: exponential improvements for quantum matrix arithmetics," arXiv:1806.01838: https://arxiv.org/abs/1806.01838

## Verdict

Sound with minor caveats. The high-level template is right, but the note is too terse about polynomial feasibility conditions and normalization/error bookkeeping.

## Line-Anchored Comments

- Lines 10-12: the "choose a function, approximate by a bounded polynomial, apply QSVT" summary is right. Add that only polynomials satisfying the QSVT parity and boundedness constraints are directly implementable.
- Line 14: "proves a lower bound on the efficiency of singular value transformation" is too vague. State what parameter is lower bounded, or remove the claim from a short summary.
- Lines 36-39: parity is not simply "even/odd depending on singular-value vs eigenvalue transform." The exact parity constraints depend on the QSP/QSVT sequence length and the block being extracted.
- Lines 47-49: the chronology should be handled carefully. Qubitization appeared as a 2016 preprint and 2019 journal article; QSVT appeared as a 2018 preprint and STOC 2019 paper.
- Line 59: Childs 2010 gives a walk/Hamiltonian spectral relation; QSVT generalizes the broader polynomial-transform viewpoint, not that specific arcsin relation by itself.

## Missing or Suggested Additions

- Add a short checklist: normalization `alpha`, block-encoding error, polynomial degree, parity/boundedness, postselection or amplitude amplification if the transformed block is not directly unitary.
