# Review: The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003)

Source note: [[The Power of Basis Selection in Fourier Sampling (Moore-Rockmore-Russell-Schulman 2003) — Paper Notes]]

## Verdict

Minor-to-major issues. The note is a useful HSP summary and captures the weak/strong Fourier-sampling distinction. A few representation-normalization and scope details need checking, and the cross-links section appears unfinished.

## Line-Anchored Comments

- Lines 23-35: The results list is good. Add which statements are fully efficient, measurement-reconstructible, and query-reconstructible; the note later defines these categories but the opening mixes them.

- Lines 37-53: The three-level reconstructibility taxonomy is valuable and should be kept.

- Lines 57-105: The affine-group representation setup is detailed. Check the semidirect-product convention against the multiplication rule; left/right cosets and conjugation conventions are easy to flip in this problem.

- Lines 108-132: The vector `(u_b)_j = 1/(p-1) omega_p^{bj}` appears unnormalized for a state vector; likely it should carry a `1/sqrt(p-1)` normalization or be explicitly a column of a projection rather than a unit vector.

- Lines 134-154: The large-order `q`-hedral result should state the exact promise on prime `q` and the relation between `q`, `p`, and `polylog(p)`. As written, it is easy to misread as applying to arbitrary large `q`.

- Lines 156-178: The measurement-reconstruction section is good. Emphasize that the hard part left open is efficient classical decoding from the sample outcomes.

- Lines 257-268: The cross-links section is effectively empty after the header. Fill it or remove it.

## Missing or Suggested Additions

- Add a convention box for `A_p`, `H_b`, left/right cosets, and the basis chosen after the nonabelian Fourier transform.
- Add a short comparison with the dihedral HSP and graph isomorphism HSP to calibrate the significance.
