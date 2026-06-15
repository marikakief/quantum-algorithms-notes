# Review: Carry-Lookahead Tree for Logarithmic-Depth Addition

Source note: [[Carry-Lookahead Tree for Logarithmic-Depth Addition]]

## Primary Sources Checked

- Draper, Kutin, Rains, and Svore, arXiv:quant-ph/0406142.

## Verdict

Minor issues. The tree construction is well summarized. The comparison language should not call Fourier-basis arithmetic a simple competing log-depth approach without qualifiers.

## Line-Anchored Comments

- Lines 12-17: the propagate/generate recurrence is clear.
- Lines 19-28: the four-phase schedule and exact Toffoli count are useful.
- Lines 42-46: the caveats are excellent and capture the real depth-versus-ancilla tradeoff.
- Line 57: "competing `O(log n)`-depth approach" for Fourier-basis arithmetic needs qualification. The addition phase can be highly parallel, but the full QFT adder depends on the QFT implementation, ancillae, and connectivity.

## Missing or Suggested Additions

- Add a note that QCLA is a parallel-prefix-style carry computation, while MAJ/UMA is a ripple computation; this helps readers choose the right mental model.

