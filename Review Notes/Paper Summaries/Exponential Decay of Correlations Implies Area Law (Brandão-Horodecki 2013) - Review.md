# Review: Exponential Decay of Correlations Implies Area Law (Brandão-Horodecki 2013)

Source note: [[Exponential Decay of Correlations Implies Area Law (Brandão-Horodecki 2013) — Paper Notes]]

## Primary Sources Checked

- Brandão and Horodecki, "Exponential decay of correlations implies area law," arXiv:1206.2947; Commun. Math. Phys. 333, 761-798 (2015).

## Verdict

Minor-to-major issues. The technical summary is good, but the "quantum advantage requires algebraically decaying correlations" conclusion is too broad, and the MPS/simulability corollaries need their approximation and circuit-geometry assumptions stated.

## Line-Anchored Comments

- Lines 9-13: good definition of exponential decay of correlations. It is important that this is an operator-correlation definition over separated regions.
- Lines 19-28: the logical picture is broadly correct for 1D pure states/gapped Hamiltonians, but do not imply the proof is a simple composition of Hastings' exponential clustering plus a black-box converse.
- Line 28: "polynomial bond dimension" needs the dependence on system size and target error. The area law gives efficient approximability, but global approximation and local-observable approximation are different statements.
- Line 30: overbroad. A computation maintaining exponentially decaying correlations in a fixed 1D geometry is simulable under tensor-network assumptions, but "quantum advantage requires at least algebraically decaying correlations" is not a model-independent statement. Non-1D geometry, mixed-state models, magic, and sampling settings complicate this slogan.
- Lines 38-46: theorem/corollary statements are useful. Keep smooth max-entropy in the main theorem; it is stronger and more precise than jumping directly to von Neumann entropy.
- Lines 50-86: proof sketch is strong and highlights why single-shot entropies enter.
- Lines 92-99: key results are okay, but "classical simulability" should be phrased as a consequence under 1D tensor-network simulation conditions, not as an automatic circuit-complexity theorem for all processes.
- Lines 104-112: comparison is good. The "exponential tightness" sentence should be tied to the general correlation-decay setting, not to gapped-Hamiltonian area laws where AKLV improves the dependence.
- Lines 117-125: caveats are good and should remain prominent.

## Missing or Suggested Additions

- Add a "scope" box: 1D, pure-state theorem; mixed-state extension has extra dependence; higher dimensions open.
- Add a precise simulability corollary with the relevant geometry and approximation model.

