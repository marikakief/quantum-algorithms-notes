# Review: Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017)

Source note: [[Quantum Algorithm for Linear Differential Equations (Berry-Childs-Ostrander-Wang 2017) — Paper Notes]]

## Primary Sources Checked

- Berry, Childs, Ostrander, and Wang, "Quantum algorithm for linear differential equations with exponentially improved dependence on precision", CMP 2017 / arXiv:1701.03684.

## Verdict

Major omissions. The note's conceptual description is right, but it is too terse for a paper summary. It lacks the main theorem, parameter definitions beyond `g`, and the structure of the Taylor/history-state linear system.

## Line-Anchored Comments

- Add a top-level title.
- Lines 8-26: good conceptual overview of turning the ODE into one larger sparse linear system.
- Lines 31-37: good warning that this is not a general time-dependent simulation framework.
- Lines 43-47: the limitations are useful, especially the `g`-factor, but they need to be tied to an explicit complexity theorem.
- Lines 49-55: good predecessor context through Taylor-series Hamiltonian simulation.
- Missing throughout: the paper's actual query complexity and dependence on `T`, sparsity, `kappa`, `g`, and `epsilon`.
- Missing throughout: the linear-system construction details: segments, Taylor order, padding/final-time readout, and how the improved QLSA produces polylog precision.
- Lines 73-81: the successor links are useful, but this note currently has more downstream context than source-paper content.

## Missing or Suggested Additions

- Add a "Key results" section with the main theorem and parameter table.
- Add the Taylor recurrence equations or at least a block description of the linear system. Without that, the note is a conceptual bookmark rather than a reviewable summary.

