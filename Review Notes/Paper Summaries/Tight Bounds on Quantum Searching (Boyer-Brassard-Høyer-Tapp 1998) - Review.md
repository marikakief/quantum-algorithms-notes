# Review: Tight Bounds on Quantum Searching (Boyer-Brassard-Hoyer-Tapp 1998)

Source note: [[Tight Bounds on Quantum Searching (Boyer-Brassard-Høyer-Tapp 1998) — Paper Notes]]

## Primary Sources Checked

- BBHT, "Tight bounds on quantum searching", arXiv: https://arxiv.org/abs/quant-ph/9605034
- ArXiv abstract confirms: exact success formula, iteration count for almost certainty, unknown-number-of-solutions algorithm, approximate counting, and lower bound.

## Verdict

Minor issues. The main formulas and unknown-`t` schedule are good. A few attributions and constants need tightening.

## Line-Anchored Comments

- Lines 19-23: four contributions

  Good match to the arXiv abstract.

- Lines 44-50: optimal iteration count and restart strategy

  The known-`t` iteration formula is right. The restart-optimization constants are more specialized; cite the section/equation or mark them as "BBHT's expected-time optimization" rather than a central theorem.

- Lines 69-71: unknown-`t` overhead

  Good. The note should mention the `t=0` case separately: the search problem with no marked item cannot certify absence with the same expected bound unless a maximum schedule/checking convention is specified.

- Lines 89-98: quantum counting sketch

  Good as a sketch, but line 95's "measure the second register" phrasing may imply postselection is essential. The later phase-estimation view treats the initial state as a superposition of the two Grover eigenvectors; no actual postselection on solution/non-solution is conceptually necessary.

- Lines 139-140: "exact tight constant later determined"

  Add the reference. The exact asymptotic constant `pi/4` is associated with later optimality work such as Zalka's optimality proof, not BBHT itself.

- Line 159: "BBBV (1996)"

  Use `BBBV (1997)` or specify the preprint date. The linked note is 1997.

## Missing or Suggested Additions

- Add a short distinction between lower bounds for finding a marked item and exact counting lower bounds, since the note flows directly into counting.
