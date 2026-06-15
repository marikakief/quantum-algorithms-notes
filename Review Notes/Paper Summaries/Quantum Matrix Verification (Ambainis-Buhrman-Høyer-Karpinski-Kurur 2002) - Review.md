# Review: Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002)

Source note: [[Quantum Matrix Verification (Ambainis-Buhrman-Høyer-Karpinski-Kurur 2002) — Paper Notes]]

## Primary Sources Checked

- Ambainis, Buhrman, Høyer, Karpinski, and Kurur, unpublished 2002 manuscript as summarized in Buhrman and Špalek, "Quantum Verification of Matrix Products," arXiv:quant-ph/0409035.

## Verdict

Minor issues. The reconstruction is reasonable and correctly labels the result as unpublished. The main edits should keep the field/ring assumptions and bounded-error subroutine structure explicit.

## Line-Anchored Comments

- Lines 1-2: good metadata caution. Since the original manuscript is unpublished, cite Buhrman-Špalek as the accessible source for the algorithm.
- Lines 9-17: the comparison table is useful. "Freivalds optimal classically" should be scoped to the black-box/entry-query verification setting.
- Lines 23-25: good assessment. This is historically important but superseded.
- Lines 35-42: the block decomposition is clear. The random vector should be over the relevant field/ring distribution used by Freivalds-style testing; `{0,1}` is a common choice but the algebraic assumptions matter.
- Lines 44-47: Grover over rows is right if each row check can be computed coherently and a violation is marked without measurement. Mention the reversible implementation cost.
- Lines 48-52: amplitude amplification over blocks is the right high-level idea. The single-block randomized verifier is bounded-error, so this is really search with a bounded-error predicate.
- Lines 58-60: the lower bound `Omega(n)` is weak. If this note is used for current status, point to the remaining gap against `n^{5/3}`.
- Lines 78-81: caveats are good, especially the unpublished-source warning and query-vs-time model distinction.

## Missing or Suggested Additions

- Add a one-line bridge to the Buhrman-Špalek improvement: the later paper's key improvement is not only a Johnson walk, but cheaper maintained data for each walk vertex.

