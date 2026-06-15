# Review: Coset Sampling via Fourier Transform

Source note: [[Coset Sampling via Fourier Transform]]

## Primary Sources Checked

- Simon SIAM page: https://epubs.siam.org/doi/10.1137/S0097539796298637
- Shor arXiv/PDF: https://arxiv.org/abs/quant-ph/9508027 and https://arxiv.org/pdf/quant-ph/9508027
- Kitaev paper metadata not yet fully source-checked in this tranche; treat Kitaev generalization claims as provisional until that note is reviewed.

## Verdict

Major issues. The Simon part is sound, but the card overstates exactness and efficiency for general HSPs and for Shor.

## Line-Anchored Comments

- Line 7: "measurement outcome is a uniformly random element of the dual subgroup"

  Correct for finite abelian HSP in the usual exact coset-state setting. For nonabelian groups, measurement outcomes are irreducible representations and matrix indices, not simply elements of a dual subgroup. The parenthetical "or analogue" is not enough to prevent overreading.

- Line 18: "Repeat O(log|G|) times, solve for H classically"

  Too broad. For finite abelian groups, polynomially many samples and efficient classical post-processing are available. For nonabelian groups, this statement is false in general. Make the step explicitly abelian.

- Line 22: "For G = Z_N (Shor's algorithm) ... multiples of N/r"

  This needs care. In order finding, the period `r` is the step size in the exponent function, and Shor uses a Fourier modulus `q` with `N^2 <= q < 2N^2`, not generally a group size divisible by `r`. The measured values are near multiples of `q/r`, and continued fractions recover `r`. Do not present it as exact `Z_N` annihilator sampling unless you add the divisibility idealization.

- Line 27: "Factoring / discrete log: hidden subgroup of Z_N or Z_N x Z_N"

  Better: order finding is cyclic period finding; discrete log can be formulated as an abelian HSP over a product group. Factoring is reduced to order finding and only then becomes a period-finding problem.

- Line 37: nonabelian caveat

  Good caveat. It should be moved higher, before the general six-step recipe is read as universal.

## Missing or Suggested Additions

- Split into "Exact finite abelian HSP template", "Simon's problem", and "Shor/order finding approximation".
