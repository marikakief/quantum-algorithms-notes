# Primorial Coprimality Sieve for Deterministic Factoring

> **Source:** Harvey-Hittmeir, arXiv:2105.11105
> **Tags:** #trick #classical-number-theory #factoring #sieving

## What it does

Reduces a deterministic factoring search by considering only residue classes modulo a small primorial that are coprime to that primorial.

## The trick

Let

$$
m=\prod_{2\le r\le B\atop r\text{ prime}} r.
$$

Before running a structured search for a prime factor $p$ of $N$, compute $\gcd(N,m)$. If this GCD is non-trivial, it gives a factor immediately. Otherwise every prime factor $p\mid N$ is coprime to $m$, so a search over $p\bmod m$ only needs to visit

$$
\sigma\in\{1,\ldots,m\}\quad\text{with}\quad \gcd(\sigma,m)=1.
$$

There are $\varphi(m)$ such residues. For primorial $m$, Mertens' theorem gives

$$
\frac{\varphi(m)}{m}
=\prod_{r\le B}\frac{r-1}{r}
=O\!\left(\frac{1}{\log B}\right).
$$

With $B\asymp \log N$, this is a $1/\log\log N$ density factor in the residue loop. In [[A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021) — Paper Notes]], this factor propagates through a balancing calculation and becomes the final $(\log\log N)^{-3/5}$ saving.

## When to reach for it

- A hidden prime factor $p$ is searched by residue classes modulo a freely chosen modulus.
- Small prime divisors can be tested and removed cheaply before the main search.
- The dominant cost is roughly proportional to the number of residue classes tested.

## Complexity

Computing $m$ for $B=O(\log N)$ and testing $\gcd(N,m)$ is polynomial in $\log N$. The saving is not from this preprocessing cost; it is from replacing $m$ candidate residues by $\varphi(m)$ candidate residues.

## Caveat

The trick gives only a log-log saving. It helps when the rest of the algorithm can preserve the reduced residue condition; it does nothing if the search problem is not organised by residues modulo $m$.

## Related notes

- [[A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021) — Paper Notes]]
- [[Lattice-Constrained Lehman Linear Combinations]]
- [[Product-Tree Bluestein Collision Search Modulo an Unknown Factor]]
