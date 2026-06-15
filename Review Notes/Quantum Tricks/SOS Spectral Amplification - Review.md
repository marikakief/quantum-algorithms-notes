# Review: SOS Spectral Amplification

Source note: [[SOS Spectral Amplification]]

## Primary Sources Checked

- Low et al., arXiv:2502.15882 / Physical Review X 15, 041016 (2025).
- Checked general SOS/spectral-amplification context against the cited 2025 follow-up arXiv:2505.01528 at the level of metadata and terminology.

## Verdict

Major issues. The high-level trick is important, but the card currently blurs phase estimation on `H` with phase estimation on a rectangular/SOS walk, and it has broken wikilink/LaTeX formatting.

## Line-Anchored Comments

- Line 7: the reduction from `lambda/epsilon` to roughly `sqrt(Delta lambda)/epsilon` needs all symbols defined. In the chemistry paper, the relevant `Delta` is not simply the physical spectral gap.
- Line 13: do not say phase estimation is run directly on `H_SA` unless `H_SA` is a proper unitary walk or block-encoded Hermitian operator in the stated construction. The spectrum-amplification procedure uses a rectangular block-encoding/walk derived from the SOS factors.
- Lines 18 and 21: the wikilink embedded inside the LaTeX subscript breaks readability and likely Obsidian rendering. Move the link outside the formula.
- Lines 24-31: the relation to ordinary block encoding is good, but the card should say that the SOS representation and lower-bound quality determine whether the method actually wins.

## Missing or Suggested Additions

- Add a clean three-line formula sequence: shifted Hamiltonian as SOS, rectangular map, amplified walk normalization. That would make the card much safer to reuse.

