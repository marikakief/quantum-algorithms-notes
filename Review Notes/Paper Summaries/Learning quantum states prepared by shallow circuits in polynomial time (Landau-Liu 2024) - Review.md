# Review: Learning quantum states prepared by shallow circuits in polynomial time (Landau-Liu 2024)

Source note: [[Learning quantum states prepared by shallow circuits in polynomial time (Landau-Liu 2024) — Paper Notes]]

## Primary Sources Checked

- Landau and Liu, "Learning quantum states prepared by shallow circuits in polynomial time", arXiv:2410.23618 / STOC 2025.

## Verdict

Minor issues. This is a clear and accurate summary. The main improvement is to state the learning model and hidden constants more sharply.

## Line-Anchored Comments

- Lines 15-17: good distinction from the 2024 shallow-circuit learning paper.
- Lines 21-25: the replacement-process summary matches the paper's abstract and is well explained.
- Lines 41-52: the local-RDM and epsilon-net search sections should emphasize that this is a classical algorithm using copies/measurements of the state, not a procedure given an explicit circuit.
- Lines 83-99: the theorem statement is helpful. Define `c` before the formulas or immediately after the first occurrence; it is the real source of the hidden constants.
- Line 99: "quasipolynomial when `d = polylog(n)`" is correct, but mention that geometry/dimension assumptions still remain.
- Lines 115-120: good caveats. Keep the warning that the theorem is structural rather than practical tomography.

## Missing or Suggested Additions

- Add a one-line comparison with the 2024 paper: this later result avoids the 2D band-slicing consistency issue by reconstructing from local replacement operations.

