# Review: Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015)

Source note: [[Simulating Hamiltonian Dynamics with a Truncated Taylor Series (Berry-Childs-Cleve-Kothari-Somma 2015) — Paper Notes]]

## Primary Sources Checked

- Berry, Childs, Cleve, Kothari, and Somma, "Simulating Hamiltonian dynamics with a truncated Taylor series", PRL 2015 / arXiv:1412.4687.

## Verdict

Major issues. The algorithmic description is clear, but the note repeatedly misstates what later QSP improved. The `log(1/epsilon)/loglog(1/epsilon)` precision dependence is not a defect to be removed; the main QSP improvement is additive separation from the time parameter.

## Line-Anchored Comments

- Lines 17-19: good high-level account. The precision dependence should be described as `log(T/epsilon)/loglog(T/epsilon)` rather than simply "logarithmic" when giving asymptotic comparisons.
- Lines 25-28: the `ln 2` segmentation explanation is excellent.
- Lines 43-57: clear PREPARE/SELECT description.
- Lines 59-69: robust OAA is described accurately and is one of the best parts of the note.
- Lines 81-85: the key-results table is good.
- Lines 89 and 141: "doubling the digits doubles the cost" is only a rough intuition; the actual truncation order is `log(1/epsilon)/loglog(1/epsilon)`.
- Lines 102-104: the QSP comparison is incorrect. Low-Chuang did not "remove the loglog factor" in the precision lower-bound sense. QSP/qubitization gives query complexity roughly `alpha t + log(1/epsilon)/loglog(1/epsilon)`, whereas Taylor gives `alpha t * log(alpha t/epsilon)/loglog(...)`.
- Lines 119-127: PREPARE/SELECT gate costs are useful, but for modern readers add that coefficient-state preparation can be improved with qROM/alias-sampling data structures in specific Hamiltonian encodings.
- Lines 170-173: the "not quite optimal" bullet should be rewritten. The issue is not the `loglog` denominator; the issue is multiplicative dependence on `T` and the Taylor truncation order.
- Line 182: "never conceptually replaced" is too strong. Qubitization/QSVT is a genuine conceptual reorganization, even if LCU/block-encoding remains the language.

## Missing or Suggested Additions

- Add a small "Taylor vs QSP" equation pair:
  `Taylor: O(T log(T/epsilon)/loglog(T/epsilon))`; `QSP: O(T + log(1/epsilon)/loglog(1/epsilon))`.
