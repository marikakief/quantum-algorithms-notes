# Review: Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001)

Source note: [[Efficient Quantum Algorithms for Some Instances of the Non-Abelian HSP (Ivanyos-Magniez-Santha 2001) — Paper Notes]]

## Primary Sources Checked

- Ivanyos, Magniez, and Santha, "Efficient quantum algorithms for some instances of the non-Abelian hidden subgroup problem", arXiv:quant-ph/0102014 / SPAA 2001.

## Verdict

Minor issues. This is a strong summary and it correctly highlights black-box group algorithms plus abelian reductions. The main recommendation is to keep encoding assumptions attached to every theorem.

## Line-Anchored Comments

- Lines 9-16: excellent metadata; the unique/nonunique encoding distinction is important.
- Lines 22-27: good problem family list.
- Lines 31-42: the recurring lift-from-normal-quotient pattern is well described.
- Lines 50-69: constructive membership via an abelian HSP kernel is clear and reusable.
- Lines 75-99: small-commutator algorithm is accurate. The equality `H1G' = HG'` plus equal intersection with `G'` is the right proof invariant.
- Lines 105-130: the elementary-2 extension test is well explained. Keep the warning that exponent 2 is essential.
- Lines 134-141: key theorems are useful. Add encoding and runtime-parameter qualifiers to each bullet, not only in the caveats.
- Lines 155-161: caveats are good and should stay.

## Missing or Suggested Additions

- Add a one-line definition of `nu(G)` near the theorem list before relying on it in the caveats.

