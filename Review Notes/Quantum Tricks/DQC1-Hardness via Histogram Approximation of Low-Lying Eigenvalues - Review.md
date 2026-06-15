# Review: DQC1-Hardness via Histogram Approximation of Low-Lying Eigenvalues

Source note: [[DQC1-Hardness via Histogram Approximation of Low-Lying Eigenvalues]]

## Primary Sources Checked

- Gyurik, Cade, and Dunjko, arXiv:2005.02607 / Quantum 6, 855 (2022).

## Verdict

Minor issues. The reduction is described cleanly. The caveat should be closer to the top.

## Line-Anchored Comments

- Lines 10-18: the binning/histogram idea is clear and correctly identifies why one-bin misplacement is tolerable.
- Line 26: good to mention the truth-table/non-adaptive query nature.
- Lines 28-29: this is the key limitation: the hardness is for general sparse PSD matrices, not directly for clique-complex Laplacians. Move this caveat into "What it does."

## Missing or Suggested Additions

- Add the normalized sub-trace problem definition in one line, since the whole reduction starts there.

