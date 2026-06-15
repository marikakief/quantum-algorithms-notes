# Review: Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024)

Source note: [[Rapid Initial State Preparation for the Quantum Simulation of Strongly Correlated Molecules (Berry, Tong, Babbush, Rubin et al 2024) — Paper Notes]]

## Primary Sources Checked

- Berry et al., "Rapid initial state preparation for the quantum simulation of strongly correlated molecules", arXiv:2409.11748 / PRX Quantum 6, 020327 (2025).

## Verdict

Minor issues. The note is technically strong. The remaining problems are mostly chronology, overlap terminology, and making the extrapolation assumptions visible.

## Line-Anchored Comments

- Lines 25-26: the `~7x` synthesis improvement is real, but it is a Toffoli-count improvement for the relevant unitary-synthesis subroutine. Do not let it read as a universal 7x reduction of total QPE cost.
- Lines 95-103: the window/filtering cost discussion is good. It would help to state whether `gamma` denotes amplitude overlap or squared overlap before using it in formulas.
- Lines 114-139: sampling versus binary search is well explained. The threshold near `0.003` should be labeled as paper/model-specific because constants matter here.
- Lines 202-210: excellent catch that the paper switches between overlap and squared overlap. Keep this; it is one of the most useful warnings in the note.
- Line 238: the comparison with earlier FeMoCo estimates should say "under the extrapolation and candidate-state assumptions" every time it is used as a validation claim.
- Line 262: check the date for Low-Kliuchnikov-Schaeffer. If using the journal/publication date, say so; otherwise the arXiv chronology is older than "2024" suggests.

## Missing or Suggested Additions

- Add a one-line "published as PRX Quantum 6, 020327 (2025)" note near the citation, since the filename and source note use 2024.

