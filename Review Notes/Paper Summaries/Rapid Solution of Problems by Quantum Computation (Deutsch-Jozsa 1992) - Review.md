# Review: Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992)

Source note: [[Rapid Solution of Problems by Quantum Computation (Deutsch-Jozsa 1992) — Paper Notes]]

## Primary Sources Checked

- Deutsch and Jozsa, "Rapid solution of problems by quantum computation", Proc. R. Soc. Lond. A 439, 553-558 (1992), DOI: https://doi.org/10.1098/rspa.1992.0167
- Royal Society/INSPIRE abstract metadata checked: https://inspirehep.net/literature/2932941
- Simon SIAM abstract for later bounded-error separation context: https://epubs.siam.org/doi/10.1137/S0097539796298637

## Verdict

Minor issues. The technical account is sound as the modern version of the algorithm, but a few historical claims should be softened.

## Line-Anchored Comments

- Lines 15 and 78: one-query certainty versus modern presentation

  These are compatible, but the note should be explicit: the 1992 paper establishes an exact quantum solution, while the textbook phase-kickback circuit in this note is the later cleaned-up formulation. Without that, readers may think the displayed circuit is literally the original exposition.

- Line 17: "the first problem demonstrating that quantum computers can be exponentially faster"

  Acceptable if immediately qualified as exact quantum versus deterministic classical query complexity. The note does qualify this at lines 19 and 62. Keep the caveat close to the claim.

- Line 70: "Introduced promise problems to quantum complexity"

  Overstated. Promise problems existed before quantum computing. Better: "gave an early and influential quantum promise problem."

- Lines 102-103: BV and Simon cross-link descriptions

  Good high-level progression, but be careful with "first quantum-vs-randomised separation" for BV. BV's full paper gives a superpolynomial oracle separation; the basic hidden-string algorithm only gives a linear query separation.

## Missing or Suggested Additions

- Add "randomized classical O(1)" caveat to the table caption, not only the prose, because many readers skim tables.
