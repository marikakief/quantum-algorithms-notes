
> **Source:** Harrow, Lin & Montanaro (2017), fixing [[The Learnability of Quantum States (Aaronson 2006) — Paper Notes|Aaronson (2006)]]; used centrally in arXiv:1711.01053 (Aaronson, Shadow Tomography)
> **Tags:** #trick #shadow-tomography #quantum-or #hypothesis-testing #sample-complexity

## What it does
Distinguishes "some $E_i$ among $E_1, \ldots, E_M$ accepts $\rho$ with probability $\ge c$" from "all $E_i$ accept $\rho$ with probability $\le c - \varepsilon$" — a quantum OR over $M$ hypothesis tests — using only $O(\log M / \varepsilon^2)$ copies of $\rho$.

## The trick

**Setting:** Two-outcome measurements $E_1, \ldots, E_M$. Unknown state $\rho$. Threshold $c$, gap $\varepsilon$.

**Promise (two cases):**
- **YES:** $\exists i$ s.t. $\operatorname{Tr}(E_i \rho) \ge c$
- **NO:** $\forall i$, $\operatorname{Tr}(E_i \rho) \le c - \varepsilon$

**Procedure (sketch):**
1. Sample a uniformly random index $i \sim [M]$.
2. Apply $E_i$ to a copy of $\rho$; observe outcome $b \in \{0, 1\}$.
3. Repeat $k = O(\log M / \varepsilon^2)$ times; aggregate results.

The key technical content is a careful rejection-sampling or importance-weighting argument (Harrow–Lin–Montanaro) that makes the test sound even when the "YES" index is rare among the $M$ options. A naive union bound argument fails because a single good index among $M$ bad ones contributes only a $1/M$ fraction to a uniform sample. The fix uses amplitude estimation or a careful bias-correction argument to boost the signal from the good index.

**Sample complexity:** $O(\log M / \varepsilon^2)$ — logarithmic in $M$, independent of $D$.

## When to reach for it
- Testing whether *any* of exponentially many hypotheses is consistent with an unknown state.
- As a subroutine inside [[Gentle Search via Quantum OR]] (binary search over measurement index sets).
- Any hypothesis-testing problem over a large discrete hypothesis class where you have copies of an unknown state.

## Complexity
$O(\log M / \varepsilon^2)$ copies. No dependence on $D$ (the state dimension). This is the core reason shadow tomography achieves polylog sample complexity.

## Caveat
- Aaronson's original 2006 version of Quantum-OR had an error; the correct version is due to Harrow, Lin & Montanaro (2017). Use their version.
- The procedure makes a collective (potentially entangled) measurement across copies; single-copy implementations recover at most $O(M/\varepsilon^2)$ in the worst case.
- The $O(\log M / \varepsilon^2)$ bound is for *decision*; reducing to search (finding the good index) requires the additional $\log^3 M$ overhead from [[Gentle Search via Quantum OR]].

## Related notes
- [[Shadow Tomography of Quantum States (Aaronson 2018) — Paper Notes]]
- [[Gentle Search via Quantum OR]]
- [[Gentle Measurement Lemma]]
