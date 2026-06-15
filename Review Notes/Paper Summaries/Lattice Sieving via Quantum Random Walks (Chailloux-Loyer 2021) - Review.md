# Review: Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021)

Source note: [[Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes]]

## Primary Sources Checked

- Chailloux and Loyer, "Lattice sieving via quantum random walks", ASIACRYPT 2021 / arXiv:2105.05608.

## Verdict

Minor issues. The note is detailed and appropriately skeptical about heuristics and QRAM/QRAQM assumptions. The main thing to improve is notation discipline around the many exponents and memory measures.

## Line-Anchored Comments

- Lines 15-27: good setup and correct heuristic-random-sphere framing.
- Lines 31-37: accurate high-level contribution and good assessment of the small but real exponent gain.
- Lines 43-71: the LSF sieve framework is well explained.
- Lines 77-87: useful recovery of the classical and Laarhoven baselines.
- Lines 95-140: the Johnson-walk cost decomposition is clear. Define `epsilon` as marked-vertex fraction near the MNRS formula to avoid collision with approximation-error notation elsewhere in the vault.
- Lines 144-193: marked-vertex thinning is the right emphasis. This is the most reusable technical idea in the paper.
- Lines 198-212: good theorem statement with separate time, QRAM, quantum memory, and classical memory. Keep those labels distinct everywhere.
- Lines 215-243: the tradeoff formulas are dense. They are valuable, but add a warning that these are exponent fits/ranges from the paper and should be checked before being quoted.
- Lines 259-264: excellent caveats. The QRAM/QRAQM caveat should also appear in the opening assessment.

## Missing or Suggested Additions

- Add a small glossary distinguishing classical memory, QRAM, QRAQM, and coherent quantum working memory. These are not interchangeable resources in lattice-sieving estimates.

