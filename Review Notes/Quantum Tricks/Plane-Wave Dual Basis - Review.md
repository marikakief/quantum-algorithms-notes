# Review: Plane-Wave Dual Basis

Source note: [[Plane-Wave Dual Basis]]

## Primary Sources Checked

- Babbush et al., *Low Depth Quantum Simulation of Electronic Structure*, arXiv:1706.00023, PRX 8, 011044 (2018): https://arxiv.org/abs/1706.00023

## Verdict

Minor issues. The card captures the key dual-basis idea well: kinetic energy is simple in momentum space and potential energy is simple in the dual real-space grid. The cost statements need tighter parameter conventions.

## Line-Anchored Comments

- Lines 7 and 24: The term-count reduction to `Theta(N^2)` is source-faithful for the plane-wave dual representation. Clarify whether `N` counts spatial orbitals, spin orbitals, or grid points, because the card later sums over spin.

- Lines 17-20: "Potential diagonal in the dual basis" is right in the discrete-variable/collocation representation. The word "approx" should be tied to basis-set/discretization error, not to a failure of diagonality inside the chosen finite representation.

- Lines 26-28: The coefficient expression is useful but should include the usual exclusions/background handling for the Coulomb `nu = 0` term in periodic charged systems.

- Lines 30-32: Good identification of the commuting measurement/simulation structure.

- Lines 43-47: These asymptotic scalings require fixed density and specific normalization conventions. The comparison to Gaussian-basis scaling is too compressed and may confuse Hamiltonian 1-norm, term count, and algorithm depth.

- Lines 51-54: Strong caveats. The periodic-boundary and pseudopotential qualifications are essential and should remain close to the "when to reach for it" section.

## Missing or Suggested Additions

- Add a parameter box: `N` grid/orbital count, `eta` electron count, volume `Omega`, density assumptions.
- Mention neutralizing background / treatment of the zero Fourier mode for Coulomb systems.
- Link explicitly to FFFT as the basis-change cost between kinetic and potential representations.
