# Incremental Eigenvalue Estimation

> **Source:** Krovi, Magniez, Ozols, Roland, arXiv:1002.2419
> **Tags:** #trick #eigenvalue-estimation #quantum-walk #search #parameter-free

## What it does
Finds a marked element without knowing the required phase estimation precision, by doubling the precision geometrically and repeating a constant number of times at each level.

## The trick
When $\mathrm{HT}^+(P,M)$ is unknown, the required precision $T = O(\sqrt{\mathrm{HT}^+})$ for eigenvalue estimation is also unknown. The fix is *incremental search*:

1. Set $t = 1$ (1-bit precision).
2. Run Search($P, M, s^*, t$) a constant $k = 50$ times.
3. If no marked vertex found, set $t \leftarrow t+1$ and repeat.

Once $t$ reaches the threshold $t_0$ where $2^{t_0} \geq 14\sqrt{\mathrm{HT}^+}$, each Search succeeds with probability $\geq 1/36$. The $k = 50$ repetitions boost the failure probability per level to $\leq 1/4$.

The total expected cost is dominated by the final level: $O(\log T) \cdot S + T \cdot (U + C)$ where $T = \sqrt{\mathrm{HT}^+}$. The geometric sum ensures earlier levels contribute only $O(1)$ multiplicative overhead.

## When to reach for it
- Any eigenvalue-estimation-based algorithm where the required precision isn't known in advance
- Quantum walk search with unknown hitting time or spectral gap
- More broadly: any "binary search over precision" where the algorithm has constant success probability once precision is sufficient

## Complexity
Same asymptotic complexity as the known-precision case ($O(\sqrt{\mathrm{HT}^+})$ walk steps), with an $O(\log T)$ multiplicative overhead in the setup cost $S$.

## Caveat
The $O(\log T)$ factor in setup cost can matter when $S \gg U + C$. In applications where setup is the bottleneck (e.g. preparing the stationary distribution), this overhead is non-trivial. Also, the constant factor from 50 repetitions per level is large, though asymptotically irrelevant.

## Related notes
- [[Quantum Walks Can Find a Marked Element on Any Graph (Krovi-Magniez-Ozols-Roland 2016) — Paper Notes]]
- [[Interpolated Walk for Quantum Search]]
- [[Phase Estimation as Eigenvalue Filter for Walk-Based Search]]
