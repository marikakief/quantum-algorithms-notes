# Near-Unitary Pairing for High-Success LCU

> **Tags:** #trick #LCU #success-probability #design-pattern
> **Source:** Childs–Wiebe arXiv:1202.5822

## What it does
Raises postselection success in [[Linear Combination of Unitaries (LCU)|LCU]] addition by pairing and combining **nearby** unitaries first.

## The trick
For the two-unitary primitive implementing $\kappa U_a + U_b$, failure scales with branch distance:
$$P_{\mathrm{fail}} \propto \|U_a-U_b\|^2\frac{\kappa}{(\kappa+1)^2}$$

So in a multi-term [[Linear Combination of Unitaries (LCU)|LCU]], do recursive pairwise merges with pairs that are close in operator norm. Keep operations coherent and defer measurements until the end.

This turns "[[Linear Combination of Unitaries (LCU)|LCU]] coefficient design" into a graph-clustering problem over unitary terms: pair close nodes first.

## When to reach for it
- Multi-term LCUs with postselection (especially subtractive LCUs)
- Product-formula sums where terms differ mainly by small time-step perturbations
- Any construction where success probability is the bottleneck

## Complexity impact
Can reduce cumulative failure from linear in number of terms to near-constant per level, if pairwise distances are controlled.

## Caveat
Requires explicit or implicit access to similarity structure between unitary branches. If terms are far apart, this trick gives little benefit.

## Related Paper Notes

- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
