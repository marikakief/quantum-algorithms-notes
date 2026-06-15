# Review: A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014)

Source note: [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]

## Primary Sources Checked

- Peruzzo et al., "A variational eigenvalue solver on a quantum processor," Nature Communications 5, 4213 (2014) / arXiv:1304.3061.

## Verdict

Minor issues, with one important comparison-table correction. The note is appropriately skeptical and historically useful, but the phase-estimation/QSVT comparison compresses too many resource models into a few entries.

## Line-Anchored Comments

- Lines 7-12: the historical framing is fine, but "launched the NISQ era" is a field-history judgment rather than a technical claim. It is acceptable in a review note, but should not substitute for the algorithmic contribution.
- Lines 17-29: the expectation-estimation description is correct at a high level. The cost formula `O(|h_max|^2 M / p^2)` is only a crude worst-case expression; more precise shot bounds depend on coefficient `L1` norm, term variances, grouping, and target failure probability.
- Lines 31-50: good concise description of the hybrid loop. Add that the variational guarantee is only an upper bound for the exact expectation of the prepared state; finite-shot estimates and hardware calibration errors can violate the apparent bound.
- Lines 54-58: the "chemical accuracy after correcting for a systematic shift" point needs emphasis. The corrected curve is not the same claim as raw hardware energies being chemically accurate.
- Lines 62-71: the claim-versus-reality table is useful and should stay. The UCC "exponential advantage" row should say that avoiding classical BCH truncation does not itself establish end-to-end quantum advantage.
- Lines 73-90: the assessment is strong, though "lasting contribution is probably sociological" is too dismissive. VQE also produced durable technical infrastructure: measurement allocation, ansatz compilation, error mitigation, and hybrid benchmarking.
- Lines 94-104: the comparison table needs repair. QPE coherence cost should scale with simulation cost and inverse precision, not `kappa` unless a specific linear-system analogy is being invoked. QSVT ground-state preparation likewise depends on block-encoding normalization, gap/filter parameters, and initial overlap.
- Lines 107-111: the point that term-by-term Pauli estimation predates VQE is good and worth keeping.

## Missing or Suggested Additions

- Add a short "resource model" paragraph before comparing VQE, QPE, and QSVT: coherent depth, total shots, precision scaling, state overlap, and Hamiltonian-simulation/block-encoding cost are distinct resources.

