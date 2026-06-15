# Review: Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003)

Source note: [[Shor's Discrete Logarithm Quantum Algorithm for Elliptic Curves (Proos-Zalka 2003) — Paper Notes]]

## Primary Sources Checked

- Proos and Zalka, "Shor's discrete logarithm quantum algorithm for elliptic curves", arXiv: https://arxiv.org/abs/quant-ph/0301141
- ar5iv rendering of the paper: https://ar5iv.labs.arxiv.org/html/quant-ph/0301141

## Verdict

Major issues in one technical subsection; otherwise strong. The high-level account is good, but the reversible Euclidean-algorithm formula has a likely transcription error and should not be left as written.

## Line-Anchored Comments

- Line 31: "around 1000 qubits"

  Correct as a headline approximation. The source abstract says a 160-bit ECC key could be broken using around 1000 qubits, compared with about 2000 qubits for a security-comparable 1024-bit RSA modulus.

- Lines 61-73: exact QFT over `Z_q` versus power-of-two QFT

  Good. The source explicitly discusses replacing the exact transform over order `q` with transforms of order `2^n`, and analyzing the success probability rather than assuming exact divisibility.

- Line 73: "success probability close to 1 in one noiseless run"

  Soften this. The source provides a detailed success-probability appendix; a single run has high constant/large probability under its parameter choices, but "close to 1" can overstate the algorithmic guarantee unless the extra register bits and post-processing conditions are specified.

- Lines 111-117: exceptional elliptic-curve additions

  This is a good and important caveat. Keep the distinction between incomplete affine formulas that fail on rare cases and complete/arithmetic-heavy alternatives.

- Lines 196-206: reversible Euclidean quotient recovery

  Line 205 is almost certainly wrong as written: `q = floor((-a - q b)/b)` is self-referential. This should be checked against the paper's notation and corrected before the note is trusted as a description of the reversible Euclidean step.

- Lines 208-218: desynchronised Euclidean algorithm

  Good conceptual summary. The "little bit of garbage" point is subtle and important: the division subroutine can tolerate branch-completion garbage because the computation is later run backward.

## Missing or Suggested Additions

- Add a note that this paper treats elliptic curves over prime fields `GF(p)`, not binary-field curves or arbitrary finite fields.
