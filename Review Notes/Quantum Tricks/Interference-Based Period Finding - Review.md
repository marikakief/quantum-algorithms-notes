# Review: Interference-Based Period Finding

Source note: [[Interference-Based Period Finding]]

## Primary Sources Checked

- Simon SIAM page: https://epubs.siam.org/doi/10.1137/S0097539796298637
- Shor arXiv/PDF: https://arxiv.org/abs/quant-ph/9508027 and https://arxiv.org/pdf/quant-ph/9508027

## Verdict

Major issues. The mechanism section is useful, but the "classical barrier" and `Z_N` exact-peak language are too broad.

## Line-Anchored Comments

- Lines 11-21: periodic coset to Fourier peaks

  Correct as an idealized exact-period statement when the subgroup divides the finite group cleanly. For Shor, the period generally does not divide the Fourier register size, so the result is concentration near multiples of `q/r`, not exact peaks at multiples of `N/r`.

- Lines 25-29: applications

  Factoring and discrete log are appropriate examples. The discrete-log map on line 27 is not quite right as written; Shor's discrete-log section uses a two-register superposition and computes `g^a x^{-b}`, then uses conditions on the measured pair. Rewrite rather than compressing into a one-line period map.

- Lines 31-35: "Classical algorithms can detect period ... only by finding collisions"

  Too strong. This is a good intuition for Simon-type black-box periods, but not a theorem for every periodic function model. Classical algorithms may exploit arithmetic structure of the function when it is not a black box. Narrow the claim to black-box functions with hidden period and no additional exploitable structure.

- Line 33: `Omega(sqrt(N/r))`

  Needs model definition. For Simon's problem, `Omega(2^{n/2})` follows from birthday-style collision reasoning. For a generic domain `{0,...,N-1}` with period `r`, the bound depends on promise, range, and access model. Do not state this formula without a theorem reference.

## Missing or Suggested Additions

- Add "exact periodicity with divisor promise" versus "approximate Fourier sampling" distinction.
