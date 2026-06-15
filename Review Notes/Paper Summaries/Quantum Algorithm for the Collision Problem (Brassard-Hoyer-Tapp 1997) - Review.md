# Review: Quantum Algorithm for the Collision Problem (Brassard-Hoyer-Tapp 1997)

Source note: [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]]

## Primary Sources Checked

- Brassard, Hoyer, and Tapp, "Quantum Algorithm for the Collision Problem", arXiv: https://arxiv.org/abs/quant-ph/9705002
- ar5iv rendering of the paper: https://ar5iv.labs.arxiv.org/html/quant-ph/9705002
- Shi, "Quantum lower bounds for the collision and the element distinctness problems", arXiv: https://arxiv.org/abs/quant-ph/0112086

## Verdict

Major issues. The main algorithm is described well, but the space-time tradeoff is repeatedly written as if BHT proved an arbitrary-algorithm lower bound. They did not; they gave an algorithmic tradeoff for their birthday-plus-Grover technique.

## Line-Anchored Comments

- Lines 11-14: headline query complexities

  Correct for the promised `r`-to-one setting, with `N = |X|`. Classical birthday search is `Theta(sqrt(N/r))`; BHT gives `O((N/r)^{1/3})` expected evaluations.

- Lines 30-40: algorithm and query count

  Good. The source theorem states expected evaluations `k + sqrt(N/k)` for the two-to-one case and the analogous `k + sqrt(N/(r k))` expression for `r`-to-one functions.

- Lines 52-56: `ST^2 >= |F(X)| = N/r`

  This is dangerous as written. In BHT this is a parameter relation for their family of algorithms, not a lower bound on all possible collision algorithms. The card `[[Birthday-Grover Space-Time Tradeoff]]` makes the stronger "any algorithm" claim; that is not supported by BHT.

- Lines 58-61: endpoints of the tradeoff

  The `S = 1` endpoint is not really the same algorithm unless one has a fixed table element/target to collide with; phrase it as the limiting "one stored value plus Grover over possible partners" version. The classical birthday endpoint is also a different purely classical strategy, not a Grover-improved point on exactly the same circuit family.

- Lines 75-77: claw finding

  Mostly right for equal-size one-to-one domains. If `|X|` and `|Y|` differ, use the source theorem's asymmetric parameters rather than quoting only `O(N^{1/3})`.

- Lines 97-99: optimality

  Correct in spirit, but cite Shi cleanly. Shi's lower bound proves tight `Omega((N/r)^{1/3})` collision queries for `r`-to-one functions; Ambainis gives the later optimal element-distinctness algorithm/lower-bound landscape.

- Line 108: hash output length

  Good cryptographic implication for generic collision search: `n` output bits give about `2^{n/3}` quantum collision work, so `3 lambda` output bits are needed for `lambda`-bit generic collision resistance.

## Missing or Suggested Additions

- Add a short "query complexity vs actual time" caveat. BHT count oracle evaluations; sorting and superposition table lookup/binary search are separate costs and become important outside the pure query model.
