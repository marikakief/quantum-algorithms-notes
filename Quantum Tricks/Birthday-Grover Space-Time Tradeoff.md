# Birthday-Grover Space-Time Tradeoff

> **Source:** Brassard, Høyer & Tapp, arXiv:quant-ph/9705002 (1997), Corollary 3
> **Tags:** #trick #collision #tradeoff #query-complexity #cryptography

## What it does

Establishes a continuous space-time tradeoff for quantum collision finding: any algorithm using space $S$ and $T$ queries must satisfy $ST^2 \geq N/r$ (for $r$-to-1 functions over domain of size $N$).

## The tradeoff curve

| Space $S$ | Queries $T$ | Regime |
|---|---|---|
| $1$ | $O(\sqrt{N})$ | Pure Grover (pick one element, search for its pair) |
| $N^{1/3}$ | $O(N^{1/3})$ | **BHT sweet spot** |
| $\sqrt{N}$ | $O(\sqrt{N})$ | Pure birthday (classical) |

The constraint $ST^2 \geq N$ means:
- Doubling the table size cuts the query count by $\sqrt{2}$
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

This is why post-quantum hash function parameters are set to $3\times$ the target security level for collision resistance (e.g., SHA-384 for 128-bit quantum security).

## Caveat

The $ST^2 \geq N$ lower bound holds in the standard oracle model for $r$-to-1 functions. For more structured problems (e.g., Simon's problem with $f(x) = f(x \oplus s)$), the structure enables exponentially better algorithms. For less structured problems (element distinctness without an $r$-to-1 promise), the tight bound is $\Theta(N^{2/3})$ even with unlimited space.

## Related notes

- [[Quantum Algorithm for the Collision Problem (Brassard-Høyer-Tapp 1997) — Paper Notes]]
- [[Classical Preprocessing plus Grover Search]]
- [[Standard Amplitude Amplification]]
- [[On the Power of Quantum Computation (Simon 1994) — Paper Notes]] — the structured case where the tradeoff is beaten exponentially
