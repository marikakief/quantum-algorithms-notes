# Review: Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240)

Source note: [[Quantum Eigenvalue Processing and Transformation for Non-Normal Matrices (arXiv 2401.06240) — Paper Notes]]

## Primary Sources Checked

- Low and Su, "Quantum Eigenvalue Processing", FOCS 2024 / SIAM Journal on Computing 2026 / arXiv:2401.06240.
- SIAM Journal on Computing metadata confirms Vol. 55, Issue 1, pages 135-215, published February 12, 2026.

## Verdict

Major issues. The note correctly identifies QEVT/QEVE as the non-normal eigenvalue analogue of the QSVT toolkit, but it appears to overstate the reduction from polynomial degree dependence to polylogarithmic degree dependence. QEVT nearly recovers Hermitian/QSVT scaling, and QSVT scaling is still degree-dependent.

## Line-Anchored Comments

- Lines 7-13: good problem framing. The distinction between singular-value and eigenvalue processing is exactly right.
- Lines 17-20: the history-state/polynomial-basis explanation is a good conceptual hook.
- Lines 34 and 50: "avoiding linear-in-degree cost" and "polylogarithmic dependence on the degree, rather than linear" are likely wrong as written. Polynomial/eigenvalue transformations still carry degree or `1/epsilon`-type dependence in the query complexity; the paper aims to nearly match the Hermitian QSVT degree scaling while paying non-normality overheads, not to make arbitrary high-degree transformations polylogarithmic.
- Lines 56-64: the Chebyshev/Fourier explanation for real spectra is good. The `O(1/d)` precision should be tied to a history length/degree `d`, which then reappears in query complexity.
- Lines 66-70: good summary of Faber-polynomial use. Add that the spectral set and conformal map data are nontrivial classical inputs.
- Lines 72-74: fast coefficient loading may be polylogarithmic for structured coefficient families, but that does not remove the transform-degree dependence from the whole algorithm. Keep those two claims separate.
- Lines 90-96: the QEVE `O(kappa_V/epsilon)` scaling is a useful corrective to the earlier "polylog degree" language.
- Lines 102-104: "strictly linear time query complexity for average-case diagonalizable operators" needs a theorem reference and parameter definitions. "Average-case" is too vague here.
- Lines 119-123: the caveats are good and should be moved closer to the main claims.

## Missing or Suggested Additions

- Add a small table separating three costs: polynomial degree / target precision, eigenbasis condition number, and spectral-region geometry. Those are the three knobs a reader needs to track.
