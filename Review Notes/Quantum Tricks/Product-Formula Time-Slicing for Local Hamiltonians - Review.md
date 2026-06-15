# Review: Product-Formula Time-Slicing for Local Hamiltonians

Source note: [[Product-Formula Time-Slicing for Local Hamiltonians]]

## Primary Sources Checked

- Lloyd, "Universal Quantum Simulators," DOI `10.1126/science.273.5278.1073`: https://pubmed.ncbi.nlm.nih.gov/8688088/
- Childs, Su, Tran, Wiebe, Zhu, "A Theory of Trotter Error," arXiv:1912.08854: https://arxiv.org/abs/1912.08854

## Verdict

Major issues in the complexity/resource wording. The core first-order product-formula idea is right, but several resource claims are too informal for a reusable trick card.

## Line-Anchored Comments

- Lines 12-18: the first-order formula is fine, but the error bound should specify the norm and local-term strengths. `O(ell^2 ||H||^2 t^2 / epsilon)` is not the same as Lloyd's local-commutator bound unless assumptions are stated.
- Line 28: the gate-count bullet drops the `ell`/commutator dependence that line 18 included. This makes the complexity look better than the displayed bound.
- Line 30: "wall-clock time proportional to `t`" is a physical parallel-control statement, not a circuit-complexity statement.
- Line 32: the higher-order scaling is missing dependence on number of terms, norms, and formula constants. It is okay as intuition, but not as a reusable bound.
- Line 35: qubitization's optimal cost should be stated with normalization, e.g. `alpha t` plus a precision term. Raw `O(t + log(1/epsilon))` only applies after normalization.

## Missing or Suggested Additions

- Add a short "resources tracked" block: number of terms, local norm, commutator norm, step count, and circuit depth under serial versus parallel scheduling.
