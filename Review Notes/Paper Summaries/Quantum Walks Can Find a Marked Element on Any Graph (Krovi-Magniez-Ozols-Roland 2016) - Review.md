# Review: Quantum Walks Can Find a Marked Element on Any Graph (Krovi-Magniez-Ozols-Roland 2016)

Source note: [[Quantum Walks Can Find a Marked Element on Any Graph (Krovi-Magniez-Ozols-Roland 2016) — Paper Notes]]

## Primary Sources Checked

- Krovi, Magniez, Ozols, Roland, "Quantum walks can find a marked element on any graph", arXiv:1002.2419, Algorithmica 2016: https://arxiv.org/abs/1002.2419
- Szegedy 2004 and MNRS 2007 checked as background for hitting-time and marked-state search comparisons.

## Verdict

Major issue: the optimal interpolation parameter is wrong. The rest of the note is strong and captures the distinction between hitting time, extended hitting time, finding versus detecting, and the role of interpolated absorbing chains. But the displayed `s*` has an erroneous square root, and that error propagates to the linked trick card.

## Line-Anchored Comments

- Lines 17-18: main result is right but should say "any reversible Markov chain", not just "any graph".

  The graph supplies the state space and adjacency support; the theorem is phrased in terms of a reversible ergodic Markov chain `P` and marked set `M`.

- Lines 45-55: two-dimensional geometric picture is useful.

  Be explicit that `|U>` and `|M>` are stationary-distribution-weighted normalized states. They are not uniform over vertices unless `P` has uniform stationary distribution.

- Lines 51 and 76: `s*` is wrong.

  From the note's own formulas, balancing `cos^2 theta(s)` and `sin^2 theta(s)` requires `(1-s)(1-p_M)=p_M`, hence `s* = 1 - p_M/(1-p_M)` when `p_M < 1/2`. The note writes `1 - sqrt(p_M/(1-p_M))`, which does not make the overlaps equal except in accidental cases.

- Lines 82-90: extended hitting time section is good.

  Add a sentence that `HT^+` is the relevant quantity for multiple marked vertices and can be strictly larger than ordinary hitting time; this is not just a proof artifact in the paper, since an explicit separation example is given.

- Lines 96-102: parameter-knowledge table is helpful.

  It would be stronger if it distinguished "walk steps" from full complexity including setup `S`, update `U`, and checking `C`. Right now the table compresses these into one column.

- Lines 118-123: comparison table has a notation typo.

  Use `|M|=1`, not `||M||=1`.

- Line 125: 2D-grid comparison is essentially right but parameter `n` should be defined.

  If `n` is the number of vertices, classical hitting time is `Theta(n log n)` and the KMO cost is `Theta(sqrt(n log n))` up to implementation factors.

## Missing or Suggested Additions

- Correct the interpolation parameter in both this source note and `[[Interpolated Walk for Quantum Search]]`.
- Add a compact derivation of `s*` immediately after the overlap formulas; it is one line and prevents the square-root error from recurring.
