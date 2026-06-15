# Review: A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996)

Source note: [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]]

## Primary Sources Checked

- Dürr and Høyer, "A Quantum Algorithm for Finding the Minimum," arXiv:quant-ph/9607014 (1996).

## Verdict

Minor issues. The note is concise and mostly accurate. The main edits should expose the oracle/comparator model and avoid making the fixed-time success analysis sound like the algorithm certifies minimality during a run.

## Line-Anchored Comments

- Lines 11-15: good problem statement and query bound. Add that the table is accessed coherently through a comparison/value oracle; this is not a speedup for a list already sitting only in classical memory without coherent access.
- Lines 25-30: the algorithm description is right. The threshold register `|y>` is classical between iterations after measurement, not a persistent quantum threshold through the whole algorithm.
- Lines 26 and 48: the timeout constants are useful. Clarify that the timeout produces constant success by Markov's inequality over expected time; it does not make the algorithm know that the current `y` is minimal.
- Lines 38-46: the probabilistic analysis is the right core. The `1/r` threshold-selection fact depends on the exponential-search subroutine returning a uniformly random marked item, so preserve that assumption if this is shortened.
- Lines 56-58: optimality is correct for comparison/query access. If values are non-distinct, define whether the output is any minimum index.
- Lines 63-71: good "why it matters" section. It may be worth distinguishing Dürr-Høyer minimum finding from amplitude estimation/optimization methods that optimize a real-valued function rather than search an explicit finite list.
- Lines 77-80: caveats are good. The approximate-minimum bullet should say this is a variant, not a theorem proved in the same form in the two-page paper.
- Lines 94-96: the `Gilyén` cross-link appears with decomposed diacritics in one target name. Check whether Obsidian resolves it.

## Missing or Suggested Additions

- Add a one-line model box: ordered values, coherent comparator/value oracle, bounded-error output of an index, and repetition by taking the best returned candidate.

