# Review: Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022)

Source note: [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]]

## Primary Sources Checked

- Lee et al., "Evaluating the evidence for exponential quantum advantage in ground-state quantum chemistry," Nature Communications 14, 1952 (2023), arXiv:2208.02199.

## Verdict

Minor issues. This is a strong summary of the paper's argument and limitations. The main correction is tonal: "negative answer" should be softened to "no evidence in the studied regimes," because the paper is deliberately evidentiary rather than a no-go theorem.

## Line-Anchored Comments

- Lines 8-10: good formulation of the EQA question. Keep the distinction between absolute error and energy-density error; it is central.
- Lines 16-18: "the answer is negative" is too strong. The paper argues there is no current evidence for generic EQA in ground-state chemistry; it does not prove generic EQA absent.
- Lines 28-32: the QPE cost expression is useful. Mention that standard repetition gives `1/S^2`, while amplitude amplification or more elaborate preparation changes the overlap dependence.
- Line 36: "improving the ansatz ... can also solve for the energy" is an important heuristic argument, but it is not a theorem. Phrase it as a tension rather than an implication.
- Lines 38 and 56: the ASP time-overlap correlation is well described. Add that this is empirical on the studied Fe-S models, not a universal ASP theorem.
- Lines 42-46: the classical-scaling discussion is correctly labeled as conjectural/empirical. Keep those labels whenever quoting polynomial scaling.
- Lines 52-64: good summary of empirical results. The DMRG "superpolynomial but not exponential" point is especially useful.
- Lines 84-91: caveats are strong and should stay near the top if this note is shortened.
- Line 140: the rapid-MPS-initial-state follow-up materially changes the state-preparation story for FeMoCo. Consider promoting it from cross-link to an "updated context" note.

## Missing or Suggested Additions

- Add a short "scope" box: ground-state energies, active-space models, heuristic classical evidence, no claim about dynamics/thermal/excited-state tasks.

