# Review: Quantum Spectral Methods for Differential Equations (Childs-Liu 2020)

Source note: [[Quantum Spectral Methods for Differential Equations (Childs-Liu 2020) — Paper Notes]]

## Primary Sources Checked

- Childs and Liu, "Quantum spectral methods for differential equations", CMP 2020 / arXiv:1901.00961.

## Verdict

Major smoothness wording issue. The spectral-method construction is well explained, but the note repeatedly implies that `C^\infty` smoothness alone gives the displayed super-exponential Chebyshev convergence. The actual precision claim depends on quantitative derivative/analyticity-type bounds, represented by parameters such as `g'`.

## Line-Anchored Comments

- Lines 7-15: good problem setup, including the boundary-value extension.
- Lines 17-29: strong conceptual summary.
- Line 27: "if the solution is `C^\infty`, the Chebyshev approximation error decreases faster than any polynomial" is not true for arbitrary `C^\infty` functions without quantitative derivative bounds. Smooth functions can have slowly decaying spectral coefficients. Tie the claim to the paper's derivative-growth assumptions.
- Lines 35-63: the Chebyshev pseudospectral linear-system construction is clear.
- Lines 66-68: revise the convergence discussion. The displayed `(e/(2n))^n` behavior requires bounds on high derivatives; it is not a consequence of bare smoothness.
- Lines 74-81: the key-results table is useful and includes the relevant `g'` dependence.
- Lines 85-95: good comparison with Berry 2014, BCO 2017, and Berry-Costa.
- Lines 97-109: the caveats are strong, but the smoothness caveat should be upgraded and moved earlier.

## Missing or Suggested Additions

- Add a short paragraph explaining `g`, `g'`, and what regularity assumptions make `n=polylog(1/epsilon)` valid. This is the central numerical-analysis condition.

