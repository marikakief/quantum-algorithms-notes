# Review: Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011)

Source note: [[Quantum Algorithms for Testing Properties of Distributions (Bravyi-Harrow-Hassidim 2011) — Paper Notes]]

## Verdict

Minor issues. The summary is strong and explains the sample-then-estimate pattern well. It needs sharper oracle-model language and less casual treatment of tightness/history for uniformity.

## Comments

- The distribution oracle is not a direct probability oracle. It is a coherent sampling/function oracle whose preimage sizes induce the distribution. State this explicitly before introducing `EstProb`.

- Classical samples and quantum queries are both used in the algorithms. The query-complexity comparison should say which accesses are counted and whether classical sampling is free or counted.

- The QRAM/membership caveat for random sets is important. It belongs near the orthogonality algorithm, not only under limitations.

- The table's "tight" labels should be sourced by problem. Orthogonality is tight via the collision lower bound in this paper; uniformity tightness depends on later lower-bound work and should be labeled as such.

- The epsilon dependence is heavily polynomial and differs between the three testers. If the note is used as a reference, constants-as-constant assumptions should be visible.

## Suggested Additions

- Add an oracle-model box: domain size, range size, sampling function, quantum query, and induced probability.
- Add a note that these are polynomial speedups for symmetric distribution properties, not exponential separations.
