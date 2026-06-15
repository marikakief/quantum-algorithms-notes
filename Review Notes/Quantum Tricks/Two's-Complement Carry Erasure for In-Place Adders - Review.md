# Review: Two's-Complement Carry Erasure for In-Place Adders

Source note: [[Two's-Complement Carry Erasure for In-Place Adders]]

## Primary Sources Checked

- Draper, Kutin, Rains, and Svore, arXiv:quant-ph/0406142.

## Verdict

Minor issues. The idea is right, but the identity proof should be made less handwavy.

## Line-Anchored Comments

- Lines 8-16: the two's-complement identity is the core. The prose "matches up to complementation" is plausible but not rigorous enough for a reusable trick card.
- Lines 18-23: the operational steps are clear.
- Lines 31-37: good complexity and modulo caveat.

## Missing or Suggested Additions

- Add a bit-level or carry-recurrence derivation showing exactly why the reverse carry computation sees the same carry string. This is the part a reader would need to trust before reusing the trick.

