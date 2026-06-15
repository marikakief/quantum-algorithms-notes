# Review: Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001)

Source note: [[Quantum Fingerprinting (Buhrman-Cleve-Watrous-de Wolf 2001) — Paper Notes]]

## Primary Sources Checked

- Buhrman, Cleve, Watrous, and de Wolf, "Quantum fingerprinting," Physical Review Letters 87, 167902 (2001), arXiv:quant-ph/0102001.

## Verdict

Minor issues. The summary is clear and gets the SMP separation right. The main edits should make the classical lower bound less hand-wavy and clarify that the lower bound on quantum message length is not just a dimension-counting slogan.

## Line-Anchored Comments

- Lines 9-15: excellent statement of the SMP model and equality bounds. Keep "without shared randomness" attached to the exponential separation.
- Lines 23-31: the fingerprint construction is accurate. Say explicitly that efficient preparation assumes the sender can compute the error-correcting codeword positions coherently/classically as part of message preparation.
- Lines 33-39: good inner-product bound.
- Lines 43-55: swap-test explanation is correct and concise.
- Lines 61-65: the intuition for why classical private-coin SMP fails is helpful, but it is not the proof. The Newman-Szegedy lower bound is an information/rectangle argument, not merely "the referee can only compare individual bits."
- Lines 76-78: the `Omega(log n)` quantum lower-bound explanation is too compressed. There are infinitely many quantum states in a fixed dimension; the rigorous route is via converting short quantum fingerprints to classical descriptions/protocols with controlled error, or via known SMP lower-bound arguments.
- Lines 84-92: the multi-copy state-comparison section is useful. "Apply a random permutation, then undo" should be phrased as a projection/test related to the symmetric subspace; otherwise the operation sounds like it does nothing.
- Lines 117-121: the comparison to BCW and Raz is accurate and helpful.

## Missing or Suggested Additions

- Add a one-line model table: private-coin SMP classical, public-coin SMP classical, quantum SMP without shared randomness.

