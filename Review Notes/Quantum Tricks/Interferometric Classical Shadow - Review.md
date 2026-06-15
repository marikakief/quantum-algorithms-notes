# Review: Interferometric Classical Shadow

Source note: [[Interferometric Classical Shadow]]

## Primary Sources Checked

- Zhao et al., arXiv:2604.07639, checked current arXiv v1.
- Huang, Kueng, and Preskill, arXiv:2002.08953, for classical-shadow background.

## Verdict

Minor issues. The Hadamard-test-as-shadow idea is clear. The complexity statement needs the usual shadow-norm/test-family caveat.

## Line-Anchored Comments

- Lines 12-29: the signed-overlap construction is well explained.
- Line 30: "provided overlaps with stabilizer states can be computed efficiently" is important; move this caveat into the complexity section too.
- Lines 43-47: `O(log(m/delta)/epsilon^2)` is not a standalone guarantee for arbitrary test states. The hidden shadow norm and classical post-processing assumptions are essential.
- Lines 49-53: good caveat about real parts. If complex overlaps are needed, state how the imaginary part would be obtained or say the card does not cover it.

## Missing or Suggested Additions

- Add the exact observable family and shadow-norm bound used in the massive-classical-data paper. That will keep this from being overgeneralized to arbitrary state readout.

