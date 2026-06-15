# Review: Quantum Search with Advice (Montanaro 2009)

Source note: [[Quantum Search with Advice (Montanaro 2009) — Paper Notes]]

## Primary Sources Checked

- Montanaro, "Quantum search with advice", TQC 2009 / Theory of Computing 2010.

## Verdict

Minor issues. This is a clear and useful note. The main thing to police is model language: the large gaps are average-case and depend on sorted advice or coherent advice-state access.

## Line-Anchored Comments

- Add a top-level title.
- Lines 18-32: excellent distinction between known-advice and unknown-advice models.
- Lines 38-48: good summary. The claim that the quantum/classical gap can be larger than the usual quadratic search gap is correct only in the average-case/advice-weighted metric; the note should repeat that qualifier in the assessment sentence.
- Lines 56-71: the geometric-block algorithm is well explained.
- Line 63: exact Grover under a "zero or one marked item" promise is fine, but a reader may need a reminder that this is not the standard unknown-number-of-solutions setting.
- Lines 89-99: good handling of the final exact-search fallback.
- Lines 127-148 and 172-184: the heavy-tail examples are useful. Double-check the exponents because negative power-law parameters are easy to misread; the qualitative regimes look right.
- Lines 200-203: strong caveats. Keep these near the top if the note is shortened.

## Missing or Suggested Additions

- Add a one-line comparison between "knowing the order of probabilities" and "knowing exact probabilities." The known-advice algorithm mainly needs the rank order, not full numerical advice.

