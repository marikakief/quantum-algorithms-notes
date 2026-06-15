# Review: Incremental Kinetic Energy Register

Source note: [[Incremental Kinetic Energy Register]]

## Primary Sources Checked

- Su et al., arXiv:2105.12767, interaction-picture circuit compilation.

## Verdict

Minor issues. This is a good trick card. The main thing to clarify is that it applies to the interaction-picture algorithm, not to qubitized kinetic-energy LCU.

## Line-Anchored Comments

- Lines 7 and 11-15: strong motivation. It clearly identifies why repeated recomputation is expensive.
- Lines 17-22: the update identities are correct and are the core reusable idea.
- Line 24: the numeric comparison is plausible and useful; if retained, mention the assumed `n_eta` and constants so the arithmetic is reproducible.
- Line 26: subtracting the energy offset at the start is a nice detail, but "for free" should mean no repeated per-step recomputation, not literally zero gates.
- Lines 36-40: good source-level cost summary.
- Lines 42-46: caveats are appropriate. The phase-gradient/timing adjustment is easy to miss and should stay.

## Missing or Suggested Additions

- Add a line contrasting this with [[Kinetic Energy as Bit-Product LCU]]: one is for interaction-picture phasing, the other for qubitized block encoding.

