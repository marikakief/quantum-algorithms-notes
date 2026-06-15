# Review: Arithmetic in the Fourier Basis

Source note: [[Arithmetic in the Fourier Basis]]

## Primary Sources Checked

- Draper, arXiv:quant-ph/0008033 (2000).

## Verdict

Minor issues. The trick is accurately described, but gate count and depth should not be conflated.

## Line-Anchored Comments

- Lines 11-21: the Fourier-basis addition mechanism is clear.
- Line 21: `O(n log n)` is a gate-count statement after AQFT-style truncation. The depth depends on the QFT implementation, rotation scheduling, and connectivity.
- Lines 25-28: "qubit count is the bottleneck" is the right use case. Do not imply this is T-count efficient on fault-tolerant hardware.
- Lines 38-42: good caveats. Move the rotation-synthesis warning into the complexity table as well.

## Missing or Suggested Additions

- Add a row for "addition step only" versus "full QFT-add-inverse-QFT" to separate the commuting phase-rotation part from the QFT cost.

