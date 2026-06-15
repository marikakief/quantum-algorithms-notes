# Review: Quantum Algorithms for Solvable Groups (Watrous 2001)

Source note: [[Quantum Algorithms for Solvable Groups (Watrous 2001) — Paper Notes]]

## Primary Sources Checked

- Watrous, "Quantum algorithms for solvable groups", arXiv:quant-ph/0011023 / STOC 2001.

## Verdict

Minor issues. The note itself is mostly accurate and avoids the overclaim that appears in some cross-links. It should be explicit that this is not a general HSP solver for solvable groups.

## Line-Anchored Comments

- Lines 9-12: good main-result statement: order, membership, subgroup equality, normality testing.
- Lines 15-27: the subnormal-series story is clear and is the right high-level explanation.
- Lines 42-53: relative order finding using `|H>` is well described. The normality condition `H triangleleft <g>H` is the crucial Shor-style period condition.
- Lines 56-78: the phase-reference conversion is subtle and well summarized. This is the most valuable technical part of the note.
- Lines 82-91: main algorithm/copy budget is clear.
- Lines 97-107: factor-group computation using coset states is an important reusable idea.
- Lines 111-123: good "why it matters" section. Add a sentence saying the paper solves group-theoretic tasks for solvable groups, not arbitrary hidden subgroup recovery.
- Lines 125-126: the retracted isomorphism claim is good scholarship and should stay.

## Missing or Suggested Additions

- Add a warning near the top: "Do not cite this as HSP over all solvable groups." It is often conflated with normal-HSP and solvable black-box group algorithms.

