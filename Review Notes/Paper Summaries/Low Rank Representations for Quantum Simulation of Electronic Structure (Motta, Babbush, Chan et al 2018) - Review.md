# Review: Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018)

Source note: [[Low Rank Representations for Quantum Simulation of Electronic Structure (Motta, Babbush, Chan et al 2018) — Paper Notes]]

## Primary Sources Checked

- Motta et al., "Low rank representations for quantum simulation of electronic structure", arXiv:1808.02625 / npj Quantum Information 7, 83 (2021).
- Kivlichan et al. 2018 fermionic swap-network context from earlier review tranche.

## Verdict

Major issues, but localized. The double-factorization explanation is mostly strong; the main problem is a comparison-table entry that attributes a generic dense-Hamiltonian capability to the fermionic swap network.

## Line-Anchored Comments

- Lines 17-23: the assessment is good and appropriately calls the rank scalings empirical. Keep that caveat near any `O(N^2 log N)` headline.
- Line 21: "chemically accurate Hamiltonian Trotter step" could be misread as a Trotter-error statement. The paper controls factorization/truncation accuracy for the Hamiltonian representation; the number of Trotter steps needed for a target dynamical error is a separate question.
- Lines 57-61: the Trotter-step formula is too compressed. As written, the product order and inter-basis rotations are hard to parse. It would help to state that each factorized Coulomb term is diagonal in its own rotated basis, so consecutive layers are connected by composed Givens rotations.
- Lines 63-65: this is the technically important part and is well stated. It correctly connects partial basis rotations to a pairwise diagonal interaction layer.
- Lines 119-124: the comparison row for [[Fermionic Swap Network]] is wrong. Kivlichan et al.'s linear-depth result applies to Hamiltonians of the `T + U + V` form with density-density interactions in the plane-wave dual basis, not arbitrary dense molecular-orbital Hamiltonians. The swap network is a circuit primitive used inside this paper, not a competing `N(N-1)/2`-gate simulator for generic four-index chemistry.
- Line 126: "treats all `O(N^2)` pairwise terms" describes the density-density plane-wave-dual setting, not generic Gaussian-basis two-electron integrals. Qualify the sentence.
- Lines 130-136: the caveats are good and should stay prominent. This note is unusually careful about empirical scaling and truncation/Trotter separation.

## Missing or Suggested Additions

- Add a sentence distinguishing this Trotter compilation from later double-factorization qubitization. The same chemistry factorization vocabulary appears in both settings but the cost drivers are different.
- Clarify whether `N` means spin orbitals or spatial orbitals in each formula; the note uses both spin-sector conventions in adjacent sections.

