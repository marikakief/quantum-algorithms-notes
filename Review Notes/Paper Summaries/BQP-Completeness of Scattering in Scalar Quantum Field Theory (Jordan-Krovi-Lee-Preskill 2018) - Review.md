# Review: BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018)

Source note: [[BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) — Paper Notes]]

## Primary Sources Checked

- Jordan, Krovi, Lee, and Preskill, "BQP-completeness of scattering in scalar quantum field theory," Quantum 2, 44 (2018); arXiv:1703.00454.

## Verdict

Minor issues. The note is clear and captures both directions of the BQP-completeness result. The main thing to keep sharp is that the instance is encoded in spacetime-dependent classical sources, not in arbitrary initial particles or graph structure.

## Line-Anchored Comments

- Lines 9-17: good problem definition. The promise gap on vacuum-to-vacuum probability is the decision-problem formulation.
- Lines 21-24: the BQP-completeness summary is accurate. "Unless BQP = BPP" is the right informal classical-hardness consequence, assuming efficient classical probabilistic approximation of the same promise problem.
- Lines 31-37: double-well qubit encoding is well explained. The perturbative nonrelativistic Hamiltonian should remain explicitly identified as an effective description.
- Lines 41-51: adiabatic passage from the vacuum is a valuable detail. The `O(n^8)` preparation time depends on the chosen error scaling and parallelization assumptions.
- Lines 55-64: two-qubit gate discussion is good. The failed naive interaction story helps explain the more elaborate six-dimensional subspace construction.
- Lines 67-71: source-description complexity and BQP membership are concise and correct.
- Lines 94-99: caveats are strong. The reliability of perturbation theory with time-dependent sources is the main mathematical qualification.
- Line 95: the coupling decreasing as `O(1/G)` is not a bug; it is central to the weak-coupling hardness interpretation. Good point.

## Missing or Suggested Additions

- Add a one-line "encoding location" comparison: graph scattering encodes the computation in the graph; this paper encodes it in external source functions.
- Add a note that the result is for a massive, gapped scalar theory; massless/conformal cases are not covered.

