# Review: Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998)

Source note: [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]

## Primary Sources Checked

- Beals, Buhrman, Cleve, Mosca, and de Wolf, "Quantum lower bounds by polynomials", FOCS 1998 / JACM 2001.

## Verdict

Minor issues. The summary gets the main polynomial-method message right. The main fixes are scope and wording: the deterministic-vs-quantum polynomial relation is for total Boolean functions in the query model, and a few later-result comments need citations or less compressed phrasing.

## Line-Anchored Comments

- Add a top-level title. This note starts directly with the source block, unlike many later notes.
- Lines 13-23: accurate description of the degree-`2T` acceptance-polynomial lemma and the exact/bounded-error lower bounds.
- Lines 25 and 52-61: the `D(f)=O(Q_2(f)^6)` statement should explicitly say "total Boolean functions in the query model." The note says this later at line 113, but the headline version is easy to overread.
- Line 27: good high-level assessment. The sentence about later tightening should separate exact-query, zero-error, and bounded-error variants; they are not the same polynomial relation.
- Line 61: "sixth-root quantum speedup" is memorable but informal. Prefer "deterministic query complexity is polynomially bounded by bounded-error quantum query complexity."
- Lines 69-84: the symmetric-function table and Paturi-based formula are useful and appear correct.
- Lines 77-78: the exact/zero-error OR statement is right and worth retaining because it prevents the common mistake of applying Grover to exact computation.
- Line 109: "exponent is at most 4 for monotone functions" needs a citation or a parenthetical attribution. It is a later result, not from this paper.
- Lines 113-114: good caveat about partial functions. This should be moved nearer the headline result.

## Missing or Suggested Additions

- Add one sentence distinguishing exact degree, approximate degree, block sensitivity, and deterministic decision-tree complexity. The note currently moves among them quickly.
- Add references for the modern exponent improvements mentioned in the caveats.

