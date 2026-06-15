# Review: Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022)

Source note: [[Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/2111.00433
- Quantum article page: https://quantum-journal.org/papers/q-2022-12-07-873/

## Verdict

Minor issues. The note is technically strong and correctly identifies the persistent Laplacian and Schur-complement block encoding as the key ideas. The fixes are mostly emphasis: normalized output, two gap promises, and the difference between persistent Betti numbers and full barcodes.

## Comments

- The note says persistent Betti numbers let one recover persistent barcodes. That is too compressed: barcode recovery requires enough rank information across a filtration, not just one normalized persistent Betti estimate.

- The two spectral promises should be part of the main theorem summary: the persistent Laplacian gap and the Schur-complement/pseudoinverse gap both matter.

- The QSVT `log(1/epsilon)` improvement is correctly described as not changing the sampling-dominated end-to-end dependence. Keep that warning close to the QSVT headline.

- The state-preparation cost depends on simplex density. The note has this under promises; put it in the problem setup too, since it is the same bottleneck as LGZ.

- The output is an additive estimate of `beta_q^{K,L}/n_q^K`. The note should avoid any phrasing that sounds like exact or multiplicative persistent Betti computation.

## Suggested Additions

- Add a small diagram of the simplicial pair `K subset L` and where the Schur complement enters.
- Add a "barcode caution" sentence: estimating many persistent Betti ranks is related to, but not identical with, constructing a persistence diagram.
