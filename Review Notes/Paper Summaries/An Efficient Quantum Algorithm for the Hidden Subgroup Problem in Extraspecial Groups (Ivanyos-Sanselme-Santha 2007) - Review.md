# Review: An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Extraspecial Groups (Ivanyos-Sanselme-Santha 2007)

Source note: [[An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Extraspecial Groups (Ivanyos-Sanselme-Santha 2007) — Paper Notes]]

## Primary Sources Checked

- Ivanyos, Sanselme, and Santha, "An efficient quantum algorithm for the hidden subgroup problem in extraspecial groups", arXiv:quant-ph/0701235 / STACS 2007.

## Verdict

Minor issues. The note captures the automorphism-action idea well. The main suggestion is to spell out the two cases `G' subset H` and `G' cap H = {1}` as a decision/reduction step.

## Line-Anchored Comments

- Lines 7-9: excellent TL;DR. The "not full nonabelian Fourier transform" point is important.
- Lines 26-37: group structure is correctly summarized.
- Lines 41-50: main-result split is clear.
- Lines 52-58: reduction to finding `HG'` is central and well stated.
- Lines 62-84: phase-labelled coset states and four-copy phase cancellation are explained well. Add a sentence saying the `j_i` are chosen after the Fourier labels `u_i` are known.
- Lines 86-90: witness-generation probability is useful. The lower bound `(p-9)/(2p)` also signals small-prime exceptional handling.
- Lines 99-107: connections and assessment are good.
- Lines 109-112: caveats are accurate.

## Missing or Suggested Additions

- Add a brief note on how `H` is recovered from `HG'` in the `G' cap H = {1}` case after the abelian HSP step.

