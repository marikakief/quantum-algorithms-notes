# Review: Order-Finding via QFT and Continued Fractions

Source note: [[Order-Finding via QFT and Continued Fractions]]

## Primary Sources Checked

- Shor arXiv/PDF: https://arxiv.org/abs/quant-ph/9508027 and https://arxiv.org/pdf/quant-ph/9508027

## Verdict

Minor issues. The mathematical skeleton is basically correct. Notation and complexity conventions need tightening.

## Line-Anchored Comments

- Line 8: "using O(log log N) quantum experiments"

  Correct in Shor's analysis if `N` is the integer/modulus and high success is required by repetition. But the card should define whether `N` is the modulus or bit length; elsewhere in the vault both conventions appear.

- Lines 12-20: continued-fraction recovery

  Correct. Add that the measured `c` is only likely to satisfy the good approximation condition; the condition is not guaranteed for every outcome.

- Line 22: success per trial

  Correct source match: Shor uses the lower bound on `phi(r)/r` to get `Omega(1/log log r)`. Mention that practical post-processing by trying nearby `c` values, multiples, or LCMs can reduce expected trials in many cases, as Shor notes.

- Line 35: per-trial complexity

  Needs explicit bit-length notation. If `n = ceil(log_2 N)`, fast modular exponentiation is `O(n^2 log n log log n)` in Shor's accounting; schoolbook reversible modular exponentiation is `O(n^3)`. Do not mix this with the number of repeated experiments.

## Missing or Suggested Additions

- Add a one-line check step: candidate `r` must be validated by testing `x^r = 1 mod N`, and if not, try convergents/multiples/LCMs.
