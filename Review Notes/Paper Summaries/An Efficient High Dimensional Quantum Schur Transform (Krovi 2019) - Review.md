# Review: An Efficient High Dimensional Quantum Schur Transform (Krovi 2019)

Source note: [[An Efficient High Dimensional Quantum Schur Transform (Krovi 2019) — Paper Notes]]

## Primary Sources Checked

- Krovi, "An efficient high dimensional quantum Schur transform," Quantum 3:122, 2019; arXiv:1804.00055.

## Verdict

Minor issues. The note captures the real contribution: a strong Schur transform with `poly(n, log d, log(1/epsilon))` scaling by moving to permutation modules. The main fixes are small precision and basis-labeling caveats.

## Line-Anchored Comments

- Lines 7-19: good Schur-Weyl setup. The basis-label convention is clear enough for a review note.
- Lines 23-31: the "first fully-detailed" phrasing is acceptable because Harrow's thesis had only a sketch of the `log d` route.
- Lines 41-45: the alphabet-reduction step needs one extra phrase: the mapping must preserve equality patterns and record the original labels, not simply send every value greater than `n` to an arbitrary element of `[n]`.
- Lines 53-65: QFTPermMod is summarized well. The role of generalized phase estimation over the Young subgroup is compressed but essentially correct.
- Lines 57-59: "controlled by `Y_T`" would be clearer as "using a uniform superposition over `Y_T` and the right-regular action"; the subgroup is not a single control value.
- Lines 67-75: the RSK/GT-basis conversion is a good inclusion. The phase/sign caveat later is important for coherent downstream uses.
- Lines 81-85: theorem statement is accurate. Keep "strong Schur transform" in the result line; weak Schur sampling had easier `log d` routes.
- Lines 101-107: caveats are strong, especially weak-vs-strong sampling and phase conventions.

## Missing or Suggested Additions

- Add a one-sentence distinction between "large local dimension `d`" and "many registers `n`"; the improvement is in `d`, not a claim of polylogarithmic dependence on `n`.
- Add a warning that applications using coherent post-Schur operations need consistent phase conventions, not just measured labels.

