# Review: Givens Rotation Slater Determinant Preparation

Source note: [[Givens Rotation Slater Determinant Preparation]]

## Primary Sources Checked

- Kivlichan et al., *Quantum Simulation of Electronic Structure with Linear Depth and Connectivity*, arXiv:1711.04789: https://arxiv.org/abs/1711.04789

## Verdict

Minor issues. The card is mostly source-faithful. The corrections are about notation and scope: an arbitrary single Slater determinant is not the same thing as every fermionic Gaussian state, and the prepared reference state is an occupation string, not the all-zero vacuum unless no particles are present.

## Line-Anchored Comments

- Line 7: The `N/2` depth claim matches the source abstract, but the card later explains that symmetries and particle number are important. Keep those qualifications close to the headline.

- Line 11: A Slater determinant with `eta` electrons is specified by an `N x eta` occupied-orbital isometry, or by an `N x N` unitary extending it. Saying "specified by `u in U(N)`" is fine but not minimal.

- Line 11: The reference state should be a fixed `eta`-electron Fock state, not implicitly the all-zero vacuum.

- Lines 13-17: The Givens rotation matrix and nearest-neighbor JW locality statement are good.

- Lines 23-28: The depth-reduction discussion is useful. The `eta - 1` and `N/2` bounds should be stated under the scheduling/symmetry assumptions used in the source rather than as generic facts for every orbital ordering.

- Line 34: "Whenever you need to prepare a fermionic Gaussian state" is too broad. This prepares number-conserving single Slater determinants; general fermionic Gaussian states with pairing/Bogoliubov structure require a more general circuit.

- Lines 49-51: Good caveats, especially the single-determinant limitation.

## Missing or Suggested Additions

- Add the distinction between single Slater determinant, number-conserving fermionic Gaussian state, and general fermionic Gaussian/Bogoliubov state.
- Add that the decomposition is classically computed from the orbital coefficient matrix.
