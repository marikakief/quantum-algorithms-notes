# Review: Quantum Arthur-Merlin Games (Marriott-Watrous 2005)

Source note: [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]]

## Verdict

Minor issues. This is a strong note. The Jordan-lemma amplification section is especially clear. The revisions are mainly about terminology and keeping QMA amplification separate from the later QIP/QMAM consequence.

## Line-Anchored Comments

- Lines 7-18: The opening packs several theorems together. Consider splitting into two bullets: QMA strong amplification and interactive-proof consequences.

- Lines 29-38: The acceptance-operator definition is good. Add that eigenvectors of `Q` are the optimal witness basis for analysis, not something Arthur computes.

- Lines 40-53: The alternating-measurement procedure is clear. The threshold expression should define `q = 1/(a-b)` before line 116, where it reappears.

- Lines 55-74: The Jordan-lemma explanation is excellent. Keep this as the conceptual core.

- Lines 78-86: The `QMA subset PP` trace trick should note that the trace is over the witness Hilbert space and becomes a GapP-computable quantity via circuit-entry sums.

- Lines 88-92: `QMA_log = BQP` depends on the usual promise-gap and uniformity conventions. Add a qualifier for logarithmic witness length with polynomial-time verification and inverse-polynomial gap.

- Lines 94-105: The `QMAM = QIP` result is well summarized. Define whether `QMAM` has three messages and public coin; this will also help fix the Watrous constant-round note.

## Missing or Suggested Additions

- Add a mini glossary: QMA, QIP(1), QAM, QMAM.
- Add a pointer that the QIP=PSPACE proof uses the single-coin QMAM normal form.
