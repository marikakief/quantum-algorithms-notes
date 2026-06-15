# Review: Quantum-Aided Compressed Sensing for Sparse State Readout

Source note: [[Quantum-Aided Compressed Sensing for Sparse State Readout]]

## Primary Sources Checked

- Wiebe, Braun, and Lloyd, "Quantum algorithm for data fitting", arXiv:1204.5242, for the sparse-readout idea.
- Cramer et al., "Efficient quantum state tomography", arXiv:1101.4366, for compressed-sensing tomography context.

## Verdict

Minor issues. The card is useful, but its "M'^2 instead of M" comparison needs assumptions stated up front.

## Line-Anchored Comments

- Lines 10-22: the support-identification plus restricted tomography story is clear.
- Lines 16-20: compressed-sensing tomography guarantees require rank/sparsity structure and suitable measurement ensembles; this is not generic tomography of an arbitrary `M'`-dimensional mixed state.
- Line 36: "exponentially better" is true only when `M'` is polylogarithmic or otherwise much smaller than `M`. The caveats later say this; move a shorter qualifier into the complexity section.
- Lines 38-43: the caveats are good and should remain.

## Missing or Suggested Additions

- Add a warning that measuring computational-basis samples identifies support according to probabilities, not amplitudes; small-amplitude but important signed components can be missed.

