# Review: Information-Theoretic Bounds on Quantum Advantage in ML (Huang-Kueng-Preskill 2021)

Source note: [[Information-Theoretic Bounds on Quantum Advantage in ML (Huang-Kueng-Preskill 2021) — Paper Notes]]

## Primary Sources Checked

- Huang, Kueng, and Preskill, "Information-theoretic bounds on quantum advantage in machine learning," Physical Review Letters 126, 190505 (2021), arXiv:2101.02464.

## Verdict

Minor issues. The note captures the central distinction between average-case prediction and worst-case property prediction. The main correction is stylistic but important: avoid personal-name ambiguity and keep computational efficiency separate from sample/query efficiency.

## Line-Anchored Comments

- Lines 15-21: good model split. This is a query/sample paper, not a runtime paper; the note says this clearly.
- Lines 25-31: the average-case versus worst-case distinction is exactly the right headline.
- Line 31: "Robert's clean foundational papers" looks like a mistaken name or private shorthand. Replace it with a neutral phrase; none of the listed authors is Robert.
- Lines 37-43: the classical simulation proof sketch is acceptable. Add that the constructed restricted classical learner is information-theoretic and may not be computationally efficient.
- Lines 53-60: good explanation of Bell measurements for magnitudes and quantum memory for signs. This is the heart of the separation.
- Lines 71-79: the polynomial overhead theorem should keep all parameters visible: output-system size `m`, quantum sample/query count, and target error.
- Lines 83-93: the `O(log(M/delta)/epsilon^4)` quantum upper bound is a clean statement. For `M=4^n`, the `O(n)` copy claim is for constant precision and constant failure probability.
- Lines 97-101: the classical lower bound should be phrased as a worst-case property-prediction lower bound allowing adaptive classical measurements. It does not contradict classical shadows for restricted/local observable families because all Paulis include high-weight incompatible observables.
- Lines 114-118: caveats are strong. Consider moving the first caveat, "query complexity not runtime," into the opening paragraph.

## Missing or Suggested Additions

- Add a short comparison with classical shadows: efficient for many low-shadow-norm observables, not for the full set of all Pauli observables at constant accuracy.

