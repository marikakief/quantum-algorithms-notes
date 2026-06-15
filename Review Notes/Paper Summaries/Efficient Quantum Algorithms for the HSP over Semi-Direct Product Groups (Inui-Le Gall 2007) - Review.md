# Review: Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007)

Source note: [[Efficient Quantum Algorithms for the HSP over Semi-Direct Product Groups (Inui-Le Gall 2007) — Paper Notes]]

## Primary Sources Checked

- Inui and Le Gall, "Efficient quantum algorithms for the hidden subgroup problem over a class of semi-direct product groups", arXiv:quant-ph/0412033 / Quantum Information and Computation 7 (2007).

## Verdict

Minor issues. The subgroup-classification-driven algorithm is summarized well. The main point to emphasize is that only the `P_{p,r}` class is solved here; dihedral/quasi-dihedral cases remain outside the result.

## Line-Anchored Comments

- Lines 9-17: good metadata and input-model distinctions.
- Lines 27-45: the classification-plus-algorithm split is clear.
- Lines 51-77: automorphism classification is useful; keep the cases separated because this is where the five families come from.
- Lines 81-99: subgroup classification of `P_{p,r}` is the central structural fact.
- Lines 101-132: good normal/nonnormal split. In the nonnormal case, the constructed abelian window is the key move.
- Lines 136-165: the `Z_{p^r}^m ⋊ Z_p` extension is correctly qualified by separated generators.
- Lines 171-173: key results are accurate.
- Lines 186-191: caveats are good. The result does not solve the hard semidirect-product benchmark cases.

## Missing or Suggested Additions

- Add a small note explaining why nonunique encoding is allowed for `P_{p,r}` but not for the broader `m`-factor extension.

