# Review: Qubitization Iterate

Source note: [[Qubitization Iterate]]

## Primary Sources Checked

- Low and Chuang, "Hamiltonian Simulation by Qubitization," arXiv:1610.06546: https://arxiv.org/abs/1610.06546

## Verdict

Major issues because the card omits normalization in its only formula-level memory hook.

## Line-Anchored Comments

- Line 8: "block-encoded / standard-form encoded" should explicitly mean an encoding of `H/alpha`.
- Line 16: using the iterate for `f(H)` requires the function to be applied to the normalized spectrum first, then rescaled in the interpretation.
- Line 20: the rotation angle is `arccos(lambda/alpha)`, not `arccos(lambda)` unless the encoded operator already has norm at most 1 and `alpha=1`.

## Missing or Suggested Additions

- Add the actual iterate definition, e.g. reflection about the prepared state composed with the signal unitary, with a note that conventions differ by order/sign.
- Add the two-dimensional invariant-subspace basis so the geometry is not just a slogan.
