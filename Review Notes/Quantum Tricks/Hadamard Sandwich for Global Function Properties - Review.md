# Review: Hadamard Sandwich for Global Function Properties

Source note: [[Hadamard Sandwich for Global Function Properties]]

## Primary Sources Checked

- Deutsch-Jozsa 1992 DOI: https://doi.org/10.1098/rspa.1992.0167
- Simon SIAM page for Fourier-sampling context: https://epubs.siam.org/doi/10.1137/S0097539796298637
- Shor arXiv for period-finding/QFT context: https://arxiv.org/abs/quant-ph/9508027

## Verdict

Minor issues. Good card, with one overgeneralization.

## Line-Anchored Comments

- Lines 10-16: amplitude formula

  Correct for the Deutsch-Jozsa phase-oracle formulation.

- Line 18: "same structure, with H replaced by QFT, detects periodicity"

  This is a useful teaching statement but should be narrowed. Simon's algorithm is exactly `H^{\otimes n}` Fourier sampling over `Z_2^n`. Shor's order finding uses QFT over a power-of-two register and an approximate continued-fraction recovery step; it is not just the same sandwich with `H` swapped for QFT.

- Lines 21-22: "any global property"

  Too broad as written. The property must correspond to an efficiently accessible Fourier coefficient or spectral feature with a measurable gap. Otherwise this wording invites false algorithm design.

## Missing or Suggested Additions

- Add examples of properties it does not help with: generic balanced-vs-nearly-balanced estimation without promise, arbitrary SAT structure, and literal database lookup.
