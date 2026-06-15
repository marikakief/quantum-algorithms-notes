# Review: Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998)

Source note: [[Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups (Roetteler-Beth 1998) — Paper Notes]]

## Primary Sources Checked

- Rötteler and Beth, "Polynomial-Time Solution to the Hidden Subgroup Problem for a Class of Non-Abelian Groups", arXiv:quant-ph/9812070 (1998).

## Verdict

Minor issues. This is a strong account of an early special-case nonabelian HSP algorithm. The note should make clear that the custom pairing/Fourier transform is engineered for this wreath product and is not a generic nonabelian Fourier transform recipe.

## Line-Anchored Comments

- Lines 9-21: good problem and group definition.
- Lines 25-35: the four-step contribution summary is clear.
- Lines 42-53: the subgroup factorization `U = (U cap N)(U cap U^t)` is the core structural lemma.
- Lines 57-76: the custom pairing is well explained. Stress that `U^perp` is not generally a subgroup.
- Lines 80-90: Fourier transform description is useful but should mention basis/order dependence.
- Lines 94-112: sampling theorem and generation probability are good.
- Lines 116-135: full algorithm is clear and correctly reduces the reconstruction to linear algebra.
- Lines 155-160: caveats are accurate and should stay.

## Missing or Suggested Additions

- Add a short comparison with the affine-group basis-selection literature: both are examples where the measurement basis is the algorithm.

