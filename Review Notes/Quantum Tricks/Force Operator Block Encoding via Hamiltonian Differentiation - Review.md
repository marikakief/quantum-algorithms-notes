# Review: Force Operator Block Encoding via Hamiltonian Differentiation

Source note: [[Force Operator Block Encoding via Hamiltonian Differentiation]]

## Primary Sources Checked

- O'Brien et al., arXiv:2111.12437 / Physical Review Research 4, 043210 (2022).

## Verdict

Major issues. The card captures the high-level trick, but the algebraic identity and normalization formula are too compressed to be safe.

## Line-Anchored Comments

- Line 7: the derivative-of-Hamiltonian framing is correct: forces are expectation values of derivative operators.
- Line 17: check the square identity. If the factor being differentiated is not Hermitian in the relevant representation, the identity needs daggers or a Hermitian embedding. As written, it looks like a real-symmetric special case presented as a general formula.
- Line 23: the displayed derivative `lambda` formula is not general enough. The THC derivative construction can introduce several terms from differentiated factors, not just a differentiated central coefficient tensor unless the notation explicitly absorbs them.
- Lines 29-34: the "reuse Hamiltonian block-encoding machinery" idea is right, but it should say which registers/data tables change for gradients.
- Line 45: if the card quotes a constant-factor overhead, say whether it applies to Toffolis, normalization, oracle calls, or all of them separately.

## Missing or Suggested Additions

- Add a minimal worked symbolic identity showing the exact assumptions: real symmetric factor, Hermitian one-body operator, or embedded non-Hermitian factor. This would prevent readers from copying an invalid derivative formula.

