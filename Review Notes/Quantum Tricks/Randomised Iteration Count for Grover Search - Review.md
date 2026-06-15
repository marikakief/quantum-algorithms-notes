# Review: Randomised Iteration Count for Grover Search

Source note: [[Randomised Iteration Count for Grover Search]]

## Primary Sources Checked

- BBHT, "Tight bounds on quantum searching": https://arxiv.org/abs/quant-ph/9605034

## Verdict

Sound with minor caveats.

## Line-Anchored Comments

- Lines 16-25: algorithm and key lemma

  Correct match to BBHT's unknown-number-of-solutions schedule and averaged success probability.

- Lines 29 and 39: total expected work

  Correct asymptotically. Add that the algorithm assumes `t > 0`; if there may be no marked item, a separate stopping/certification procedure is needed.

- Line 45: `lambda in (1, 4/3)`

  Correct as a BBHT-style parameter range. Good to keep the paper's `6/5` choice.

## Missing or Suggested Additions

- Add "checks candidate with one oracle call after measurement" to make the find/verify loop explicit.
