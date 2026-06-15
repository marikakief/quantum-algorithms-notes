# Review: Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025)

Source note: [[Polynomial Time Classical Versus Quantum Algorithms for Representation Theoretic Multiplicities (Panova 2025) — Paper Notes]]

## Primary Sources Checked

- Panova, "Polynomial time classical versus quantum algorithms for representation theoretic multiplicities," arXiv:2502.20253v2. The arXiv page lists v2 from October 18, 2025, TQC 2025 proceedings / Computational Complexity.
- Larocca and Havlicek, arXiv:2407.17649v5, for the corrected target of the comparison.

## Verdict

Minor issues. The note is accurate and well balanced: it treats Panova as a correction to proposed quantum-speedup regimes, not as a proof that all multiplicity problems are classically easy.

## Line-Anchored Comments

- Lines 11-25: good setup. The emphasis on unary partition encoding is important for all polynomial-time claims.
- Lines 29-31: the assessment is right. The paper removes broad low-dimensional regimes but leaves cases where all dimensions are superpolynomial while ratios remain polynomial.
- Lines 37-45: the `aft` parameter is explained clearly. This is the conceptual hinge of the Kronecker theorem.
- Lines 49-66: the Jacobi-Trudi/multi-LR reduction is compressed but coherent. It may help to say explicitly that bounded tail size is what keeps the inner sums polynomial.
- Lines 70-92: the plethysm lattice-counting section is useful. Keep "basic plethysm" in the heading; this is not a full solution for arbitrary plethysm coefficients.
- Lines 96-97: the bounded-aft stabilization argument is plausible as written, but it is the most specialized part of the paper. A reader may need the exact theorem statement before reusing this paragraph.
- Lines 105-120: theorem and proposition statements are strong. The distinction between the concrete bounded-tail parameter and the dimension-implied `4k^2` substitution should stay.
- Lines 122-135: good to list both plethysm algorithms. The fixed-`d`, fixed-length result and the polynomial-Specht-dimension result are different regimes.
- Lines 150-154: caveats are excellent. The constants are classification-scale, not practical.

## Missing or Suggested Additions

- Add a "does not settle" box: high-dimensional balanced-ratio Kronecker/plethysm regimes remain possible targets.
- Add a cross-link back to Larocca-Havlicek's v5 note wherever "refutes conjecture" is mentioned, because the original paper has already been revised around this point.

