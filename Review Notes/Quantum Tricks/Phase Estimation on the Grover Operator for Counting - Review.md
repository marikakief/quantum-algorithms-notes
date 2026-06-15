# Review: Phase Estimation on the Grover Operator for Counting

Source note: [[Phase Estimation on the Grover Operator for Counting]]

## Primary Sources Checked

- BBHT search/counting sketch: https://arxiv.org/abs/quant-ph/9605034
- BHT quantum counting: https://arxiv.org/abs/quant-ph/9805082
- BHMT amplitude estimation: https://arxiv.org/abs/quant-ph/0005055

## Verdict

Minor issues. The conceptual mechanism is right; the complexity line is muddled.

## Line-Anchored Comments

- Lines 12-22: eigenphase mechanism

  Correct. The uniform starting state decomposes into the two Grover eigenvectors, and either eigenphase gives `theta` up to sign.

- Line 27: "Monte Carlo mean estimation"

  This is a later amplitude-estimation application. It is fine as a use case, but not part of the original BHT quantum-counting result.

- Line 33: `O(sqrt(N)/epsilon)` for relative error in estimating `sqrt(t/N)`

  This needs rewriting. Quantum counting with phase register size `P` uses `O(P)` Grover iterations and gives an error bound in `t` of order `sqrt(tN)/P + N/P^2`. Relative error in `t` costs `O(sqrt(N/t)/epsilon)`. Additive error in amplitude/probability has a different `O(1/epsilon)` statement.

- Line 37: "precision in t degrades when t is close to 0 or N"

  Near `t=0`, resolving nonzero `t` is hard. Near `t=N`, the derivative of `sin^2 theta` also vanishes but the complement can be counted instead if the oracle can be inverted. Mention complement counting.

## Missing or Suggested Additions

- Add BHT's explicit error bound from the paper note: `|t - t~| < 2*pi*sqrt(tN)/P + pi^2 N/P^2` with probability `>= 8/pi^2`.
