# Review: Quantum Hermite Transform

Source note: [[Quantum Hermite Transform — Paper Notes]]

## Primary Sources Checked

- Jain, Iyer, Somma, Bao, and Jordan, "Efficient Quantum Hermite Transform," arXiv:2510.04929v1. I checked the arXiv page: the paper was submitted October 6, 2025 and the current listed version is v1.

## Verdict

Minor-to-major issues. The note is energetic and mostly careful, but it should be made more version-aware and less sweeping in its analogy to the QFT. The transform result is real, but the application advantages are query-model results under Gaussian access assumptions.

## Line-Anchored Comments

- Lines 2-6: add a source/status line in the same style as the rest of the vault. This is a 2025 v1 preprint, not yet a settled textbook primitive.
- Line 13: "Gaussian analogue of the QFT" matches the paper's abstract, but "exponential speedup over classical" needs a model qualifier. As with QFT, the circuit transform can be exponentially smaller than writing the full classical transform output; this does not automatically imply an exponential end-to-end advantage for every downstream task.
- Lines 19-27: the transform definition and QFT caveat are good. The note should emphasize discretization and low-energy truncation more strongly because the continuum Hermite functions are not finite-dimensional basis states.
- Lines 33-41: the fast-forwarding summary is plausible, but the `O(log^2 N)` statement needs its exact dependence on simulation time/error and the low-energy promise if it is going to be reused outside this note.
- Lines 43-49: the four-step pipeline is useful. The constant-overlap state-preparation claim and fixed-point amplification cost should be kept tied to the paper's formal theorem.
- Lines 51-57: good Hermite-sampling caveat. The distortion parameter `kappa` can dominate the general-function runtime and should be near any algorithmic advantage claim.
- Lines 60-69: the advantage table needs query-model labels in every row. These are not unconditional lower bounds for computational-geometry workloads.
- Lines 73-87: the computational-geometry speculation is appropriately separated from proved results. Keep it framed as open questions, not implied applications.
- Lines 90-95: caveats are strong. Move them higher if this note becomes a central index entry, because they are the protection against overclaiming.

## Missing or Suggested Additions

- Add a "reviewed against arXiv v1" line and revisit after later versions.
- Add one paragraph distinguishing three claims: efficient discrete transform circuit, query advantages for Gaussian property testing/learning, and speculative computational-geometry relevance.

