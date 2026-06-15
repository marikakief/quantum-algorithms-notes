# Review: Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004)

Source note: [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes]]

## Primary Sources Checked

- arXiv: https://arxiv.org/abs/cond-mat/0407066

## Verdict

Minor issues. The PEPS construction and motivation are well explained. The main correction is to avoid saying the method is simply polynomial-time in `N` and `D` without immediately attaching the approximate-contraction caveat.

## Line-Anchored Comments

- Lines 13-23: Strong summary. The note correctly identifies PEPS as the higher-dimensional tensor-network analogue of MPS.

- Lines 31-41: The area-law explanation is good. Add that PEPS can represent area-law entanglement by construction, but that does not imply every area-law state is efficiently optimizable or contractible.

- Lines 45-55: The contraction discussion is accurate and important. It should be moved closer to the headline result because approximate contraction is the central computational bottleneck of PEPS.

- Lines 57-64: The variational and time-evolution algorithms are stated clearly. Emphasize that the generalized eigenvalue problem uses an approximate environment; conditioning of the norm matrix can be a practical issue.

- Lines 79-82: "Computational cost is polynomial in `N` and `D`" is too optimistic unless qualified as the cost of the proposed approximate contraction/optimization at fixed environment bond dimension. Exact PEPS contraction is #P-hard in general, as the caveats later mention.

- Lines 99-103: Good caveat. This should be part of the main result, not only a limitation.

- Lines 105-106: The statement about 2D critical systems is broadly reasonable but should be careful: area-law violations and PEPS efficiency depend on the model; PEPS can be powerful in practice without a general guarantee.

## Missing or Suggested Additions

- Add a notation key distinguishing physical dimension `d`, PEPS bond dimension `D`, and boundary/environment bond dimension `D_f`.

- Add a cross-link to classical-hardness notes if the vault has one for PEPS contraction or tensor-network contraction.

