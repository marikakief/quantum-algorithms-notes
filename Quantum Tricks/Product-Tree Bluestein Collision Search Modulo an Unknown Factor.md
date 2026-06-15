# Product-Tree Bluestein Collision Search Modulo an Unknown Factor

> **Source:** Harvey-Hittmeir, arXiv:2105.11105
> **Tags:** #trick #classical-number-theory #factoring #polynomial-arithmetic

## What it does

Turns many possible babystep/giantstep collisions modulo an unknown prime divisor of $N$ into polynomial evaluation plus GCD tests.

## The trick

Suppose $N$ is either prime or semiprime, and we have

- babysteps $1,\alpha,\ldots,\alpha^{\kappa-1}\in\mathbb Z_N$;
- giantsteps $v_1,\ldots,v_n\in\mathbb Z_N$;
- the promise that, if $N=pq$, some collision may hold modulo $p$ or modulo $q$ even if it does not hold modulo $N$.

Build the product polynomial

$$
f(x)=\prod_{h=1}^n (x-v_h)\in\mathbb Z_N[x]
$$

using a product tree. Then evaluate the geometric progression

$$
f(1), f(\alpha), \ldots, f(\alpha^{\kappa-1})\pmod N
$$

using Bluestein's algorithm. If a collision $v_h\equiv \alpha^i\pmod p$ exists, then

$$
f(\alpha^i)\equiv 0\pmod p,
$$

so

$$
\gcd(N,f(\alpha^i))
$$

reveals a non-trivial factor unless the value is $0\pmod N$. In the latter case, scan the individual $v_h-\alpha^i$ values and take GCDs to split $N$.

## When to reach for it

- A collision is guaranteed modulo an unknown factor of a composite modulus, not necessarily modulo the full modulus.
- The babysteps form a geometric progression.
- You need deterministic batch detection rather than testing every pair separately.

## Complexity

For $n,\kappa=O(N)$ in the Harvey--Hittmeir setting, the subroutine costs

$$
O(n\lg^3 N+\kappa\lg^2 N)
$$

bit operations. The product-tree term builds $f$; the Bluestein term is the fast evaluation on the powers of $\alpha$.

## Caveat

The method needs enough order in $\alpha$ to keep babysteps distinct over the relevant range. It also assumes the giantsteps are not already equal to babysteps modulo $N$; those exact matches are handled separately before this subroutine is called.

## Related notes

- [[A Log-Log Speedup for Exponent One-Fifth Deterministic Integer Factorisation (Harvey-Hittmeir 2021) — Paper Notes]]
- [[Lattice-Constrained Lehman Linear Combinations]]
- [[Primorial Coprimality Sieve for Deterministic Factoring]]
