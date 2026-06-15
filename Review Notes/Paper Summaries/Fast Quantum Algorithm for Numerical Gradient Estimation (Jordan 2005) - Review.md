# Review: Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005)

Source note: [[Fast Quantum Algorithm for Numerical Gradient Estimation (Jordan 2005) — Paper Notes]]

## Primary Sources Checked

- Jordan, "Fast Quantum Algorithm for Numerical Gradient Estimation," Physical Review Letters 95, 050501 (2005), arXiv:quant-ph/0405146.

## Verdict

Minor-to-major issues. The note explains the phase-kickback mechanism well, but the "one query for all derivatives to n bits" headline needs the smoothness, scaling, and oracle-precision assumptions attached.

## Line-Anchored Comments

- Lines 11-16: the query advantage is real, but the statement is too clean. It assumes a smooth function, a local grid around the target point, bounded derivatives, appropriate scaling of the phase, and enough output precision.
- Lines 25-28: the oracle should be described as evaluating the function near the chosen base point and adding a scaled fixed-point value. The displayed `ceil(f(delta))` hides the offset and scaling that prevent modular wraparound.
- Lines 31-39: excellent explanation of Fourier-basis kickback for arithmetic oracles.
- Lines 46-53: the product-state explanation is good. It holds when the linear approximation dominates; Hessian terms entangle/spread the Fourier peaks.
- Lines 59-65: the error-analysis summary is useful, but the comparison with classical central differences should be phrased as an asymptotic precision tradeoff, not a universal practical comparison.
- Lines 73-75: add "query complexity" to the table. Total gate complexity and function-evaluation precision still scale with dimension and desired accuracy.
- Lines 81-83: good explanation of why this is Bernstein-Vazirani-like.
- Lines 89-91: the higher-derivative caveat is too compressed. The statement "same as classical" depends on the derivative order and oracle model; cite the paper's exact recursion result if keeping it.
- Lines 92-93: practical-impact caveat is right and should remain.

## Missing or Suggested Additions

- Add a parameter box for grid side length, function-value precision, derivative bounds, and failure/error probability. This is the difference between the ideal one-query slogan and the actual numerical algorithm.

