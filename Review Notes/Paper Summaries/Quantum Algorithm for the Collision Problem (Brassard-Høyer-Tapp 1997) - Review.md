# Review: Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997)

Source note: [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]]

## Primary Sources Checked

- Brassard, Høyer, and Tapp, "Quantum Algorithm for the Collision Problem," LATIN 1998, arXiv:quant-ph/9705002.

## Verdict

Minor issues. The note captures the birthday-table plus Grover idea very well. The main fixes are to tighten the promise/model language and not present the space-time tradeoff inequality as a lower bound.

## Line-Anchored Comments

- Lines 9-14: good statement of the `r`-to-one promise. Make clear this is a collision-finding promise problem, not general element distinctness.
- Lines 20-22: the "beautifully simple" description is accurate and helpful.
- Lines 30-34: the algorithm is right. If the chosen table has an internal collision, the algorithm halts before the Grover stage; otherwise the number of external colliders is controlled.
- Lines 38-46: the query optimization is correct under the `r`-to-one promise. Keep the `(r-1)` factor out of asymptotic formulas only after saying `r` is at least 2.
- Lines 52-61: the tradeoff should be phrased as this algorithm's achievable tradeoff, roughly `T = O(S + sqrt(N/(rS)))`. The displayed `ST^2 >= N/r` reads like a lower bound, but here it is mainly the balancing curve in the Grover-dominated regime.
- Lines 65-77: the claw-finding extension is good but assumes comparable domain sizes in the simplified `N^{1/3}` statement. Keep the `N,M` asymmetry visible.
- Lines 87-91: good comparison with Simon and later element distinctness. The BHT row should always carry the `r`-to-one promise.
- Lines 95-99: optimality discussion is fine. Distinguish collision finding under a promise from collision-vs-one-to-one decision lower bounds if this is expanded.
- Lines 105-108: the hash-security implication is correct as the generic quantum collision attack story.

## Missing or Suggested Additions

- Add a one-line distinction from Ambainis element distinctness: BHT gets `N^{1/3}` because the promise guarantees many paired preimages; without that promise the tight bound is `N^{2/3}`.

