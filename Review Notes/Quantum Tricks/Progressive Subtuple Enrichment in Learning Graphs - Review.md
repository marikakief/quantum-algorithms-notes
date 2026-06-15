# Review: Progressive Subtuple Enrichment in Learning Graphs

Source note: [[Progressive Subtuple Enrichment in Learning Graphs]]

## Primary Sources Checked

- Belovs and Lee, "Quantum Algorithm for k-distinctness with Prior Knowledge on the Input", arXiv: https://arxiv.org/abs/1108.3022
- Belovs, "Learning-Graph-Based Quantum Algorithm for k-distinctness", arXiv: https://arxiv.org/abs/1205.1534

## Verdict

Minor issues. The trick is summarized well. It should note that this 2011 promised construction was followed by an unconditional 2012 learning-graph algorithm.

## Line-Anchored Comments

- Lines 7-16: staged enrichment idea

  Good. This is the correct conceptual reason the learning graph beats the naive Johnson-walk exponent under the promise.

- Lines 18-24: speciality reduction

  Good. The card correctly identifies `O(n/r_{\ell-1})` as the useful speciality improvement for extending existing subtuples.

- Lines 31-40: cost expression

  Good for the Belovs-Lee promised construction. Add that the `r_i` scale choices are for fixed `k` and the constants are not uniform in `k`.

- Lines 42-43: caveat

  Good. Typical-vertex concentration is not optional; it is the proof burden.

## Missing or Suggested Additions

- Add a "later update" line: Belovs 2012 removes the prior-knowledge assumption for `k`-distinctness using a modified learning graph with the same exponent.
