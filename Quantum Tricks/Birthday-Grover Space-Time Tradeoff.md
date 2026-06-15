
> **Source:** Brassard, Høyer & Tapp, arXiv:quant-ph/9705002 (1997), Corollary 3
> **Tags:** #trick #collision #tradeoff #query-complexity #cryptography

## What it does

Describes the space-query interpolation achieved by the BHT birthday-plus-Grover collision algorithm. It is an algorithmic tradeoff for that construction, not a universal space-time lower bound.

## The tradeoff curve

| Space $S$ | Queries $T$ | Regime |
|---|---|---|
| $1$ | $O(\sqrt{N/r})$ | Limiting one-stored-target Grover search |
| $(N/r)^{1/3}$ | $O((N/r)^{1/3})$ | **BHT sweet spot** |
| $\sqrt{N/r}$ | $O(\sqrt{N/r})$ | Classical birthday endpoint |

For the BHT family, when the Grover phase dominates,

$$
S T^2 \approx N/r = |F(X)|.
$$

This means:
- Doubling the table size cuts the Grover query count by about $\sqrt{2}$
- The cube root is the unique point where $S = T$

## When to reach for it

- Choosing parameters for [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes|quantum collision finding]] given memory constraints
- Setting hash function security parameters for post-quantum cryptography
- Understanding the resource tradeoffs in hybrid classical-quantum algorithms
- Any optimisation problem that combines a classical data structure with Grover search

## Cryptographic application

For an $n$-bit hash function:
- Classical security: $\Theta(2^{n/2})$ (birthday bound)
- Quantum security (BHT sweet spot): $\Theta(2^{n/3})$
- To get $\lambda$-bit quantum collision resistance: need $n \geq 3\lambda$ output bits

This is the generic-query rule of thumb behind using roughly $3\lambda$ output bits for $\lambda$ bits of quantum collision resistance.

## Caveat

Shi's lower bound proves $\Omega((N/r)^{1/3})$ quantum queries for promised $r$-to-one collision finding. That is a query lower bound matching the BHT sweet spot, not an `ST^2` lower bound. For more structured problems, such as Simon's promise $f(x)=f(x\oplus s)$, algebraic structure enables exponentially fewer queries. For less promised problems such as element distinctness, the tight query complexity is $\Theta(N^{2/3})$.

## Related notes

- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]]
- [[Classical Preprocessing plus Grover Search]]
- [[Standard Amplitude Amplification]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — the structured case where the tradeoff is beaten exponentially
