# Review: Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012)

Source note: [[Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) — Paper Notes]]

## Primary Sources Checked

- Ambainis and Montanaro, "Quantum algorithms for search with wildcards and combinatorial group testing", Quantum Information and Computation 2014 / arXiv 2012.

## Verdict

Minor issues. The summary is strong and gives the right separation between the wildcard and group-testing results. There is one corrupted formula in the source note and a few model caveats should be kept prominent.

## Line-Anchored Comments

- Add a top-level title.
- Lines 13-35: good problem definitions for wildcard search and combinatorial group testing.
- Lines 37-53: accurate high-level result summary, including the open gap for combinatorial group testing.
- Lines 63-89: good PGM/subset-state explanation.
- Line 162: the formula contains a control-character corruption before `inom`; this should be `\binom nk^{-1/2}` or, better, `\binom{n}{k}^{-1/2}`.
- Lines 119-137: the random-isolation plus Bernstein-Vazirani explanation is good.
- Lines 193-194: good lower-bound summary. It would help to say "positive/weighted adversary" if linking to the adversary-method notes.
- Lines 210-212: the caveats are exactly the right ones: the group-testing bounds are not tight, the measurement implementation is not the focus, and the query count is expected worst-case-input cost.

## Missing or Suggested Additions

- Add a short note distinguishing Las Vegas expected-query algorithms from bounded-error fixed-query algorithms, since both styles appear in the summary.

