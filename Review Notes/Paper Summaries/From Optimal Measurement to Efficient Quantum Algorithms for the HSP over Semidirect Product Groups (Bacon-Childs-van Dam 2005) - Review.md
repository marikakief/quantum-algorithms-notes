# Review: From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005)

Source note: [[From Optimal Measurement to Efficient Quantum Algorithms for the HSP over Semidirect Product Groups (Bacon-Childs-van Dam 2005) — Paper Notes]]

## Primary Sources Checked

- Bacon, Childs, and van Dam, "From optimal measurement to efficient quantum algorithms for the hidden subgroup problem over semidirect product groups", arXiv:quant-ph/0504083 / FOCS 2005.

## Verdict

Minor issues. The note correctly identifies the PGM-to-matrix-sum reduction as the conceptual core. The main thing to sharpen is that "optimal measurement" does not by itself mean "efficient algorithm"; efficiency depends on solving the matrix sum problem.

## Line-Anchored Comments

- Lines 9-17: good family definition. The prime `p` assumption matters for the cyclic-subgroup reduction.
- Lines 21-31: the four-part summary is accurate and nicely scoped.
- Lines 35-36: good reduction statement; clarify that this is after reducing to cyclic subgroups of the relevant form.
- Lines 40-48: the hidden-state formula is dense but useful. It may need a short prose bridge before the notation.
- Lines 52-65: excellent matrix-sum taxonomy. Keep the dihedral/subset-sum example as the warning case.
- Lines 69-74: PGM definition is correct. Mention that implementation requires coherent solution/counting information for matrix sums, not merely a classical decision procedure.
- Lines 80-86: the table is useful. Say fixed `r` explicitly for the `Z_p^r ⋊ Z_p` polynomial-time claim.
- Lines 97-103: caveats are good. In particular, growing `r` changes the Gröbner-basis story.

## Missing or Suggested Additions

- Add one sentence contrasting this with Kuperberg: PGM is optimal for the state-discrimination ensemble, while Kuperberg gives a different sieve-based feasible route for dihedral hidden shift.

