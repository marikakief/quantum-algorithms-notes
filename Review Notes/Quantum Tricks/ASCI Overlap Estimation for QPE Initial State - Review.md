# Review: ASCI Overlap Estimation for QPE Initial State

Source note: [[ASCI Overlap Estimation for QPE Initial State]]

## Primary Sources Checked

- Lee et al., arXiv:2208.02199 / Nature Communications 14, 1952 (2023).
- Tubman et al., arXiv:1809.05523 for selected-CI state-preparation context.

## Verdict

Minor issues. The method is well described, but the classical-cost and "ground state not needed" statements need caveats.

## Line-Anchored Comments

- Line 7: "without needing the ground state explicitly" is directionally right, but ASCI is still constructing an approximate selected-CI representation of the target state. Say "without exact diagonalization of the full Hilbert space."
- Lines 15-24: the overlap-estimation workflow is clear. It should specify whether the reported overlap is with a determinant, a CSF, or a selected-CI vector.
- Line 41: ASCI cost as roughly `O(M N^2)` is too schematic. Determinant generation, ranking, Hamiltonian application, and diagonalization costs can dominate depending on implementation and active space.
- Lines 52-58: the QPE relevance is right: an overlap witness matters only if the corresponding state can be prepared efficiently enough.

## Missing or Suggested Additions

- Add a warning that high overlap with a selected-CI vector does not by itself give a low-cost quantum state-preparation circuit.

