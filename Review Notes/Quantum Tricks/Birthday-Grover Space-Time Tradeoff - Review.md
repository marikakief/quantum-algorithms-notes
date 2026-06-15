# Review: Birthday-Grover Space-Time Tradeoff

Source note: [[Birthday-Grover Space-Time Tradeoff]]

## Primary Sources Checked

- Brassard, Hoyer, and Tapp, "Quantum Algorithm for the Collision Problem", arXiv: https://arxiv.org/abs/quant-ph/9705002
- Shi, "Quantum lower bounds for the collision and the element distinctness problems", arXiv: https://arxiv.org/abs/quant-ph/0112086

## Verdict

Needs rewrite. The card turns BHT's algorithmic tradeoff into a universal lower bound. That is a substantial conceptual error.

## Line-Anchored Comments

- Line 7: "Establishes ... any algorithm"

  Wrong. BHT establish that their technique yields a continuum of algorithms with space `S` and expected query count `T` satisfying the appropriate relation. They do not prove that every quantum collision algorithm must obey this space-time lower bound.

- Lines 11-20: tradeoff table

  The table is useful as intuition for the BHT technique, but it should be labeled "achieved by birthday/Grover interpolation" rather than "the tradeoff curve" for all algorithms.

- Lines 17-19: `ST^2 >= N`

  This should be rewritten. For `r`-to-one functions the relevant scale is `N/r = |F(X)|`; for `r=2` this is `Theta(N)`, so suppressing constants is okay, but the direction/meaning must be algorithmic, not lower-bound.

- Lines 30-35: cryptographic application

  Good. Generic quantum collision search costs about `2^{n/3}` for an `n`-bit hash output, so `3 lambda` output bits are the right generic collision-resistance rule of thumb.

- Line 39: "even with unlimited space"

  The element-distinctness comparison is muddled. Element distinctness has tight query complexity `Theta(N^{2/3})`; "unlimited space" is not the right distinguishing phrase, because the problem promise is different.

## Missing or Suggested Additions

- Add a separate lower-bound paragraph: Shi proves an `Omega((N/r)^{1/3})` query lower bound for collision finding. That is a query lower bound, not the `ST^2` statement currently on the card.
