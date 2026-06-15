# Review: Rectangular Block-Encoding for Spectrum Amplification

Source note: [[Rectangular Block-Encoding for Spectrum Amplification]]

## Primary Sources Checked

- Low et al., arXiv:2502.15882 / Physical Review X 15, 041016 (2025).

## Verdict

Minor issues. The card correctly identifies the non-square/rectangular block-encoding viewpoint, but one displayed complexity formula needs more precise energy notation.

## Line-Anchored Comments

- Lines 7-15: the motivation is right: spectrum amplification uses an SOS structure rather than the ordinary direct block encoding of `H`.
- Line 24: the complexity should use the shifted SOS energy above the chosen lower bound, not an ambiguous physical `E_j`. Write something like `E_j - E_SOS` or define the symbol as the relevant nonnegative SOS eigenvalue.
- Line 31: two calls to the underlying SELECT/PREPARE machinery may be right for the constructed walk, but name the exact walk/block-encoding product. Otherwise readers may think every rectangular block encoding doubles all costs.
- Lines 39-45: keep the caveat that the amplified normalization only helps if the SOS block encoding itself is efficient.

## Missing or Suggested Additions

- Add a diagram or short equation showing the rectangular map `A` with `A^\dagger A` encoding the shifted Hamiltonian. That is the conceptual core.

