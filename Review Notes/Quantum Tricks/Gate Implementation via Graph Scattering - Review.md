# Review: Gate Implementation via Graph Scattering

Source note: [[Gate Implementation via Graph Scattering]]

## Primary Sources Checked

- Childs, "Universal computation by quantum walk", arXiv:0806.1972: https://arxiv.org/abs/0806.1972

## Verdict

Minor issues. The core idea is correct: finite graph widgets have momentum-dependent scattering matrices that implement gates at a chosen momentum. The card should be more careful about channel ordering and about the distinction between encoded gates and physical local gates.

## Line-Anchored Comments

- Line 12: `2^n` parallel wires is correct.

  Add that the computational basis is encoded in wire labels. A gate on logical qubits is a scattering transformation among many wires, not a local physical interaction on two nearby qubits.

- Line 14: "if `T(k0)` is unitary"

  More precisely, the no-reflection condition plus correct relative phases/effective lengths are needed so a wavepacket, not just a plane wave, implements the intended unitary.

- Lines 22-26: composition formula should be treated cautiously.

  The order of `T_1`, `T_2`, and reflection matrices depends on left/right scattering conventions. The norm bound is the important takeaway; avoid inviting readers to memorize the displayed formula without convention labels.

- Line 32: formula-evaluation reference is related but not the same construction.

  AND-OR/NAND-tree algorithms also use scattering/spectral ideas, but Childs 2009's universality widgets are a distinct graph-scattering construction.

- Lines 40-42: caveats are good.

  The low success probability is due to wavepacket spreading and the need to start far from the widgets; it is not a failure of the gate widgets at the design momentum.

## Missing or Suggested Additions

- Add the target momentum `k=-pi/4` and the three-widget universal set directly in the trick card, not only in the paper summary.
