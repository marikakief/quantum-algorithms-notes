# Review: Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026)

Source note: [[Exponential Speedups in Fault-Tolerant Processing of Quantum Experiments (Kannan-Putterman-Cotler 2026) — Paper Notes]]

## Primary Sources Checked

- Kannan, Putterman, and Cotler, "Exponential speedups in fault-tolerant processing of quantum experiments," arXiv:2605.02057 (submitted May 3, 2026).

## Verdict

Minor-to-major issues. The conceptual framing is strong and current, but there is at least one literal formula corruption in the note and several theorem statements should be labelled as architectural noisy-quantum separations, not classical-vs-quantum speedups.

## Line-Anchored Comments

- Lines 9-18: excellent problem framing. Keep the distinction between raw physical processing and uploaded logical processing in the opening table.
- Lines 42-51: the main message is right. Add "relative to unuploaded adaptive strategies" next to both exponential-speedup claims so the title is not read as a classical-vs-quantum separation.
- Lines 81-100: good description of the surface-code upload theorem. The input channel is initially a general local effective channel and only becomes depolarising after twirling/for the learning analysis; do not collapse these too early.
- Lines 104-115: the uploaded-oracle equality is useful but cryptic. Define `NBQP` or avoid the class notation; otherwise readers will not know whether this is noisy BQP, nonuniform BQP, or paper-specific shorthand.
- Lines 131-152: the deconvolved observable explanation is good. Emphasize that inverse-noise deconvolution is unbiased but can have exponentially growing variance.
- Lines 191-204: there is a corrupted LaTeX command at line 200: it should be `\frac`. This is a concrete source-note defect.
- Lines 225-233: the lower-bound asymptotic is useful, but the `2^{n/2}` cap matters. Keep it in any shortened summary.
- Lines 239-260: the uploaded upper-bound expressions are easy to miscopy. Before moving them into another note, verify the exponent and whether the estimator is for generic moment estimation or the special distinguishing task.
- Lines 297-317: good inclusion of the exact shallow-shadow condition. Keep the small-`lambda` simplification separate from the general condition.
- Lines 334-346: caveats are strong and should remain. In particular, transduction and model noise are not engineering afterthoughts; they are assumptions of the theorem.

## Missing or Suggested Additions

- Add a one-line status box: "2026 v1; upload-first fault-tolerant quantum processing versus raw noisy quantum processing."
- Add a small glossary for `lambda`, `lambda'`, `eta`, input noise, injection noise, and raw physical circuit noise. The note currently uses several noise symbols in close succession.
