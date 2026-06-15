# Review: On the Power of Quantum Computation (Simon 1994)

Source note: [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]]

## Primary Sources Checked

- Simon, "On the Power of Quantum Computation", FOCS 1994 / SIAM J. Comput. 26, 1474-1483 (1997), DOI: https://doi.org/10.1137/S0097539796298637
- SIAM abstract and references checked: https://epubs.siam.org/doi/10.1137/S0097539796298637
- Shor's historical discussion checked: https://arxiv.org/pdf/quant-ph/9508027

## Verdict

Minor issues. The algorithmic reconstruction is good. Main fixes are historical precision and avoiding a too-clean HSP/Shor equivalence.

## Line-Anchored Comments

- Line 21: "first exponential speedup for a computational (not just decision) problem"

  Simon's paper is still an oracle promise problem, and the SIAM abstract frames it as distinguishing two classes of functions. It is fine to say the hidden string can be recovered, but the headline should be "first exponential bounded-error quantum versus probabilistic classical oracle separation", not "computational problem" without qualification.

- Lines 57-63: sampling success

  Good. The probability statement is the standard one. It would help to say that `s=0` is the injective case and requires a slightly different decision formulation; the hidden-string recovery discussion mainly assumes nonzero `s`.

- Lines 81-91: Simon to Shor comparison

  This is good pedagogically but too exact in the table. Shor's order finding is not just "coset sampling over Z_N" in the same exact finite-group way, because the QFT register size is a convenient power of two and the period need not divide it. Add "morally/structurally" or mention approximate Fourier sampling plus continued fractions.

- Lines 97-99: oracle separation table

  Good, but revise the BV row to avoid saying "polynomial separation" if the intended BV paper result is the recursive superpolynomial separation. The basic hidden-string BV problem is polynomial; the recursive BV oracle result is superpolynomial.

## Missing or Suggested Additions

- Add a sentence that Simon's problem is the abelian HSP over `Z_2^n`, but the paper predates the later unified HSP vocabulary.
