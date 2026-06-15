# Medium-Prime-Factor Targeting by One ECPP Round

> **Source:** Cheng, arXiv:math/0301179
> **Tags:** #trick #primality-proving #ECPP #number-theory

## What it does

Uses a single ECPP-style elliptic-curve reduction to move a primality proof from an arbitrary candidate $n$ to a nearby probable prime $n'$ whose $n'-1$ has a chosen kind of factor.

## The trick

Instead of using ECPP recursively until the proof reaches a tiny prime, stop after finding an elliptic curve $E/\mathbb Z_n$ with point count

$$
|E|=\omega n',
$$

where $\omega$ is fully factored and $n'$ is a probable prime satisfying the ECPP size condition

$$
n'>(\sqrt[4]{n}+1)^2.
$$

Add a side condition to the usual ECPP search: require $n'$ to lie in a useful arithmetic subfamily. In Cheng's paper, $n'$ is required to be **good**, meaning that $n'-1$ has a prime factor

$$
\log^2 n'\le r\le 2\log^2 n'.
$$

Once $E$, $P$, and $n'$ are found, the ECPP proposition says that a certificate for $n'$ lifts to a certificate for $n$. The point is that $n'$ can now be certified by a faster specialised test.

## When to reach for it

- You have a strong algorithm for a dense subfamily but need a certificate for arbitrary inputs.
- The standard recursive reduction is expensive, but one reduction can land in the target subfamily with acceptable probability.
- The reduction output has some randomness or distributional freedom, as ECPP curve orders do heuristically inside the Hasse interval.

## Complexity

In Cheng's use, one reduction is heuristically $\tilde O(\log^4 n)$ with fast multiplication, assuming ECPP curve-order heuristics and enough density of good primes in the Hasse interval. The side condition costs an extra factor roughly inverse to the target-subfamily density.

## Caveat

This is only as good as the distributional claim for the reduction outputs. If the target subfamily is sparse or badly distributed in the reachable interval, the method may be slower than ordinary ECPP.

## Related notes

- [[Implementing the Asymptotically Fast Version of the Elliptic Curve Primality Proving Algorithm (Morain 2005) — Paper Notes]]
- [[Primality Proving via One Round in ECPP and One Iteration in AKS (Cheng 2003) — Paper Notes]]
- [[Single-Congruence AKS Certificate over a Binomial Modulus]]
- [[Zero-Divisor Density Heuristic for Medium Prime Factors]]
