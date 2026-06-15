# Review: QROM-Loaded Givens Rotation Networks

Source note: [[QROM-Loaded Givens Rotation Networks]]

## Primary Sources Checked

- Lee et al., arXiv:2011.03494.
- Kivlichan et al., arXiv:1711.04789 for fixed Givens networks.

## Verdict

Minor issues. The card is conceptually useful but understates the distinction between loading angles and synthesizing/applying coherent rotations from those angles.

## Line-Anchored Comments

- Lines 5-8: the "coherently controlled basis rotation" statement is accurate and useful.
- Lines 15-20: the construction is right at a high level. The last step should distinguish inverse QROM from measurement-based cleanup, since coherent angle registers may be needed through inverse rotations.
- Line 16: `K = O(N)` Givens rotations is context-dependent. A generic single-particle unitary on `N/2` orbitals has `O(N^2)` Givens rotations; the THC construction uses structured rotations associated with selected THC vectors. State the relevant setting.
- Line 22: the cost `O(b_theta N)` is not just a small add-on. It is often the dominant constant-factor penalty of THC. The caveat at line 40 says this; the main complexity section should too.
- Line 24: good contrast with static Slater determinant preparation. Keep this; it is the card's most useful warning.
- Lines 33-36: `Gamma=O(N^2)` and total `O(b_theta N)` again assume THC rank and coarse-grained precision behavior. Mention those assumptions.
- Line 40: good caveat. It should note that arbitrary coherent rotations may be implemented through phase-gradient/Hamming-weight/CORDIC-style subroutines, so the primitive is more than a QROM lookup.

## Missing or Suggested Additions

- Add a short "fixed compiled rotations vs data-dependent rotations" comparison table. This would prevent readers from importing the cheap classical-angle Givens cost into a qubitized SELECT oracle.

