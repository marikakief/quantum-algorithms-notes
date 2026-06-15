# Review: Quantum Fourier Transform Circuit

Source note: [[Quantum Fourier Transform Circuit]]

## Primary Sources Checked

- Shor, arXiv:quant-ph/9508027, for QFT in factoring/order finding.
- Coppersmith, "An approximate Fourier transform useful in quantum factoring", quant-ph/0201067 version of the 1994 report.
- Kahanamoku-Meyer et al., arXiv:2505.00701, for modern depth/locality context.

## Verdict

Major issues. The card is a useful hub, but its gate notation and depth table are misleading.

## Line-Anchored Comments

- Lines 11-13: notation problem. Calling `R_j` a Hadamard is nonstandard and conflicts with the usual use of `R_k` for phase rotations. Rename the Hadamard layer or define the convention more carefully.
- Lines 27-33: the phase derivation has unclear summation limits and an undefined `l`. This should be rewritten or removed; it is too easy to misread as a proof.
- Lines 35-37: the AQFT summary is broadly right, but the cutoff should be tied to a target error, not just "negligible."
- Lines 48-51: the depth table is too pessimistic and too model-free. With all-to-all gates and parallel scheduling, standard exact QFT depth is linear, not necessarily `O(n^2)`; with nearest-neighbor routing it changes again. State the connectivity/scheduling model.
- Lines 53-57: the caveats are good. Add that the output bit reversal is often handled by relabeling rather than physical SWAPs.

## Missing or Suggested Additions

- Add a modern resource table separating gates, depth with all-to-all parallelism, depth on a line, ancillae, and measurement/feedforward variants.

