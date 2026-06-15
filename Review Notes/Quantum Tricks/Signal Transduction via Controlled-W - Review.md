# Review: Signal Transduction via Controlled-W

Source note: [[Signal Transduction via Controlled-W]]

## Primary Sources Checked

- Low and Chuang, "Optimal Hamiltonian Simulation by Quantum Signal Processing," arXiv:1606.02685: https://arxiv.org/abs/1606.02685

## Verdict

Sound but underdeveloped.

## Line-Anchored Comments

- Lines 9-11: the controlled-`W` construction in the `|+>, |->` basis is a useful memory hook, but the card should spell out the resulting one-qubit rotation matrix or phase convention.
- Line 13: "Add single-qubit phase shifts" is too terse for a reusable trick card. The whole QSP construction is the alternation of signal rotations and phase rotations.
- Line 18: "any eigenphase/eigenvalue transform workflow" is too broad. The signal must satisfy the polynomial-transform constraints and the target must be expressible as an allowed QSP/QSVT polynomial.

## Missing or Suggested Additions

- Add a two-line derivation for an eigenstate `W|u> = e^{i theta}|u>`, showing exactly how the ancilla rotation depends on `theta`.
