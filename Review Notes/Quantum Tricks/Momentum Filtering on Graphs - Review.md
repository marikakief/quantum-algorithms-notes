# Review: Momentum Filtering on Graphs

Source note: [[Momentum Filtering on Graphs]]

## Primary Sources Checked

- Childs, "Universal computation by quantum walk", arXiv:0806.1972: https://arxiv.org/abs/0806.1972

## Verdict

Sound with minor caveats. The filter/separator story matches the source. This card mostly needs notation cleanup and a stronger warning that filtering is part of an asymptotic wavepacket construction.

## Line-Anchored Comments

- Line 12: line-graph eigenvalue convention is right up to sign.

  For adjacency walk on the infinite line, plane waves have eigenvalue `2 cos k`; group velocity follows from the chosen time-evolution sign.

- Line 14: filter transmits both `-pi/4` and `-3pi/4`.

  Correct and important. The later separator is not optional.

- Line 16: "`m_d = log Theta(m^2)`"

  Rewrite as `m_d = Theta(log m)` filter widgets, chosen to make leakage inverse-polynomial in the circuit size.

- Line 18: effective-length numbers should be source-tied.

  These are highly specific constants. Keep them if checked against the paper; otherwise say "different effective lengths" and leave exact values to the source note.

- Line 34: caveat is good.

  Add that the arrival-time separation only works if the wires are long enough and the input wavepacket has sufficiently narrow momentum spread.

## Missing or Suggested Additions

- State explicitly that filtering reduces reflection/error away from the design momentum; it does not prepare a sharply localized computational basis state by itself.
