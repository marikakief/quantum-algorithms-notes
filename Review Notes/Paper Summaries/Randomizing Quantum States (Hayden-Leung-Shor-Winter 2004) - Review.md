# Review: Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004)

Source note: [[Randomizing Quantum States (Hayden-Leung-Shor-Winter 2004) — Paper Notes]]

## Verdict

Minor issues. The note is technically solid and explains the key reduction in private-channel key length. The main revisions are to sharpen security notions and avoid making approximate encryption sound composable against arbitrary reference systems without qualification.

## Line-Anchored Comments

- Lines 7-18: The "factor of 2 disappears" summary is good. Add "for the paper's approximate randomization criterion" in the first paragraph, because perfect private quantum channels and composable encryption use stronger notions.

- Lines 25-36: The operator-norm definition is correct and important. Explain why this is stronger than trace-norm closeness for fixed input states but still not the same as diamond-norm encryption against an adversary holding a purification.

- Lines 38-48: The proof-technique sketch is good. The union bound is over net points/projections used to control the operator norm; a little more detail would help readers see why `d log d` rather than `d^2` unitaries appear.

- Lines 52-58: The Holevo-information bound should be tied to the exact security criterion and ensemble setting. Avoid implying it covers all cryptographic composability questions.

- Lines 59-63: The data-hiding summary is accurate at the scaling level. Add that the hidden dimension is `p = Theta(d/log d)` up to parameters, so the hidden qubits are `log d - O(log log d)`, not exactly `log d`.

- Lines 99-104: The purification-accessible adversary caveat is essential. Move a shortened version near the first security claim.

## Missing or Suggested Additions

- Add a small comparison table: trace norm, operator norm, diamond norm, and what each means operationally here.
- Mention that later approximate designs give more explicit constructions than Haar-random unitaries, but with their own parameter tradeoffs.
