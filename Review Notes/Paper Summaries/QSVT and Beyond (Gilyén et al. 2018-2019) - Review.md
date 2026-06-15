# Review: QSVT and Beyond (Gilyen et al. 2018-2019)

Source note: [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]

## Primary Sources Checked

- Gilyen, Su, Low, and Wiebe, "Quantum singular value transformation and beyond: exponential improvements for quantum matrix arithmetics", STOC 2019 / arXiv:1806.01838.

## Verdict

Minor omissions. The note is a good high-level summary of the QSVT worldview, but it is too compressed for a foundational paper and overgeneralizes the lower-bound statement.

## Line-Anchored Comments

- Add a top-level title.
- Lines 8-12: good core summary. The note correctly emphasizes that algorithm design becomes block-encoding plus approximation theory.
- Line 10: QSVT transforms singular values. For Hermitian block-encodings this can implement eigenvalue transformations through the usual Hermitian embedding/sign conventions, but that distinction should be stated.
- Line 14: "proves a lower bound on the efficiency of singular value transformation" is too broad. Say which model and which transformation/query lower bound is meant; it does not make every QSVT-derived algorithm automatically optimal.
- Lines 18-23: useful ingredient table.
- Lines 34-40: good constraints list. Add the complementary-polynomial/existence condition, because boundedness and parity alone are not the whole implementation story in every formulation.
- Lines 51-59: "References within this paper" includes later or differently dated notes; split references from cross-links.
- Lines 72-78: the downstream links are helpful, but the foundational note itself should include at least one concrete theorem statement and one concrete example, such as QLSP inversion or Hamiltonian simulation.

## Missing or Suggested Additions

- Add a compact theorem box: input `(alpha,a,epsilon)` block-encoding, polynomial degree `d`, output block-encoding of `P^{SV}(A/alpha)` using `d` uses of the block-encoding and inverse.
