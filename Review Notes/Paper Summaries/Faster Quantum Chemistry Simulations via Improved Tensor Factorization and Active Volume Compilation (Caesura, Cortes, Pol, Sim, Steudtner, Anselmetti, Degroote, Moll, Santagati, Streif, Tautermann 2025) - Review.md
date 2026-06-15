# Review: Faster Quantum Chemistry Simulations via Improved Tensor Factorization and Active Volume Compilation (Caesura, Cortes, Pol, Sim, Steudtner, Anselmetti, Degroote, Moll, Santagati, Streif, Tautermann 2025)

Source note: [[Faster Quantum Chemistry Simulations via Improved Tensor Factorization and Active Volume Compilation (Caesura, Cortes, Pol, Sim, Steudtner, Anselmetti, Degroote, Moll, Santagati, Streif, Tautermann 2025) — Paper Notes]]

## Primary Sources Checked

- Caesura et al., "Faster quantum chemistry simulations on a quantum computer with improved tensor factorization and active volume compilation", arXiv:2501.06165 (2025), with related DOI 10.1103/yngp-5fpm.

## Verdict

Major issues. The note captures the paper's ingredients, but it conflates BLISS-THC with other symmetry/compression tricks and needs stricter hardware-comparison language.

## Line-Anchored Comments

- Lines 19-23: the "two orders of magnitude" claim should be explicitly tied to the paper's comparable-device assumptions. It is a runtime claim under active-volume/interleaving photonic assumptions, not a universal Toffoli-count reduction.
- Line 19: BLISS shifts are symmetry/operator shifts that preserve energies in a fixed symmetry sector. "Annihilates energy eigenvalue differences" is not the right phrasing; the shifts change the Hamiltonian outside the target sector while leaving target-sector energies invariant up to known constants.
- Line 23: "100x speedup over prior art" should name the prior-art baseline, hardware model, and molecule. The paper focuses heavily on P450; readers should not generalize the number to arbitrary chemistry instances.
- Lines 84-89: the lambda comparisons are good, but "outperforms THC" should be benchmark-limited unless the paper proves a general inequality.
- Lines 91-98: the comparison to spectrum-amplification/DFTHC results mixes different active spaces and different algorithms. The note already hints at this; make it a headline caveat in the table caption.
- Lines 114-116: the reusable-ideas links look wrong. BLISS-THC is not the same as "Symmetry-Adapted Block Encoding via QROAM Data Reduction", and DFTHC is not the same idea as BLISS-THC. Link to a BLISS/symmetry-shift card if one exists, or leave this unlinked.

## Missing or Suggested Additions

- Add a small decomposition of the speedup into factorization improvement, active-volume layout, and photonic/hardware scheduling. Otherwise readers will attribute hardware-layout gains to Hamiltonian factorization alone.

