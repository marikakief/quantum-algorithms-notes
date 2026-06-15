# Review: Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003)

Source note: [[Polynomial Degree vs. Quantum Query Complexity (Ambainis 2003) — Paper Notes]]

## Primary Sources Checked

- Ambainis, "Polynomial degree vs. quantum query complexity", FOCS 2003 / JCSS 2006.

## Verdict

Minor issues. The note captures the superlinear degree-vs-query separation and the weighted adversary composition theorem. Some historical wording should be tightened to avoid mixing exact degree, approximate degree, and later adversary developments.

## Line-Anchored Comments

- Add a top-level title.
- Lines 9-11: the central question is right, but state explicitly that bounded-error query complexity should be compared with approximate degree, while exact query complexity is compared with exact degree.
- Lines 17-21: good summary. "Spalek & Szegedy (2004)" should be checked; the standard citation for the equivalence paper is 2006.
- Lines 27-43: the base function and iterated-function construction are useful. The deterministic-query claim `D(f)=3` should include enough detail to verify the decision tree.
- Lines 45-64: the weighted adversary definition is clear. This is the best part of the note.
- Lines 68-83: good explanation of the multiplicative composition and the `2.5^d` lower bound.
- Lines 89-99: the key-results comparison table is helpful.
- Line 115: correct for positive/weighted adversary methods, but say "positive-weight" to avoid conflict with the later negative/general adversary.
- Line 121: the later-development sentence is useful but too compressed. Separate negative adversary tightness, approximate-degree separations, and the sensitivity theorem into separate bullets if this is revised.

## Missing or Suggested Additions

- Add one sentence explaining why this separation is not a separation between polynomial degree and deterministic query complexity: the latter is already known to be polynomially related for total Boolean functions.

