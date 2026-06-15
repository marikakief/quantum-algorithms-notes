# Review: Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean-Babbush-Lin et al. 2019)

Source note: [[Discontinuous Galerkin Discretization for Quantum Simulation of Chemistry (McClean, Babbush, Lin et al 2019) — Paper Notes]]

## Primary Sources Checked

- McClean et al., "Discontinuous Galerkin discretization for quantum simulation of chemistry," NJP 22, 093015 (2020) / arXiv:1909.00028.

## Verdict

Minor issues. The note is careful and correctly frames DG as basis engineering that changes the Hamiltonian representation feeding existing simulation algorithms.

## Line-Anchored Comments

- Lines 20-29: the main contribution is accurately stated. The "15-20 atoms" crossover should be marked as an empirical hydrogen-chain result, not a universal molecule-size threshold.
- Lines 55-71: the SVD blocking construction is clear. Mention that the DG functions inherit locality from the primitive partition but are not literally discontinuous grid delta functions.
- Lines 91-99: the empirical scaling table is useful. Avoid presenting fitted exponents as theorem-level asymptotics.
- Lines 121-149: good coverage of both swap-network and qubitization implications. Keep Trotter depth, `lambda`, integral count, and QROM/PREPARE costs separate.
- Lines 164-174: the caveats are strong and important. The lack of a rigorous truncation-error theorem for SVD tolerance is the main technical limitation.
- Lines 186-195: references are useful, but some "what provides" comments are really vault cross-links. This is fine for internal use; for publication split references from later influence.

## Missing or Suggested Additions

- Add a small schematic: primitive diagonal basis -> active-space orbitals -> per-block SVD -> DG block-local Hamiltonian. It would make the construction much easier to scan.
