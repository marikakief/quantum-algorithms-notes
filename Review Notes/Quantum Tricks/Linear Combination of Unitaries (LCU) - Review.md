# Review: Linear Combination of Unitaries (LCU)

Source note: [[Linear Combination of Unitaries (LCU)]]

## Primary Sources Checked

- Childs and Wiebe, arXiv:1202.5822: https://arxiv.org/abs/1202.5822
- BCCKS truncated Taylor, arXiv:1412.4687: https://arxiv.org/abs/1412.4687

## Verdict

Major issues. This is a foundational card, and it needs the coefficient/sign and success-probability details to be correct.

## Line-Anchored Comments

- Lines 8-10: PREPARE with `sqrt(beta_j)` assumes nonnegative coefficients. For signed or complex coefficients, prepare amplitudes from `|beta_j|/s` and absorb phases/signs into the controlled unitary branches.
- Line 10: the nested wikilink inside `SELECT(V)` makes the displayed operation hard to read. Keep the math plain.
- Line 14: `|0^perp>` should be written as an ancilla-orthogonal garbage state with the system included; otherwise it looks unnormalized and detached from the input.
- Line 16: success probability is not simply `1/s^2` for an arbitrary nonunitary target and arbitrary input. The amplitude prefactor is `1/s`; the probability is `||tilde U |psi>||^2/s^2`, which is near `1/s^2` only when the target is near unitary on the relevant subspace.
- Line 25: the normalization should be `s = sum_j |beta_j|` in the general complex-coefficient case.
- Line 28: OAA can boost deterministically only under its uniform-success/near-unitarity conditions. It is not an unconditional fix for every LCU.

## Missing or Suggested Additions

- Add the modern block-encoding statement: the circuit is an `(s, a, 0)` block-encoding of `sum beta_j V_j`, up to implementation error.
