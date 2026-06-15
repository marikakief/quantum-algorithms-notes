# Semigroup Index-Period Detection by Tail-Skipping Period Finding

> **Source:** Childs and Ivanyos, arXiv:1310.6238
> **Tags:** #trick #semigroups #period-finding #discrete-log

## What it does

Finds the index $t$ and period $r$ of a cyclic subsemigroup $g,g^2,\ldots$ even though the sequence has a nonperiodic tail.

## The trick

For a finite semigroup element $g$, the sequence of powers has a tail followed by a cycle:
$$
g,g^2,\ldots,g^{t-1},g^t,g^{t+1},\ldots,g^{t+r-1},\qquad g^{t+r}=g^t.
$$
Prepare a long exponent superposition
$$
\frac{1}{\sqrt M}\sum_{j=1}^M |j\rangle |g^j\rangle,
$$
with $M>N^2+N$ when $N\geq t+r$. Measuring or discarding the second register lands in the cycle with probability at least $1-N/M$, leaving an $r$-periodic state in the first register:
$$
\frac{1}{\sqrt L}\sum_{j=0}^{L-1}|x_0+jr\rangle.
$$
Now ordinary [[Order-Finding via QFT and Continued Fractions]] recovers $r$ with constant success probability.

Once $r$ is known, define
$$
\gamma(g^j)=
\begin{cases}
1 & g^{j+r}=g^j,\\
0 & g^{j+r}\neq g^j.
\end{cases}
$$
The list $\gamma(g),\gamma(g^2),\ldots,\gamma(g^N)$ is monotone: $t-1$ zeros followed by ones. Binary search gives $t$.

## When to reach for it

- A black-box associative process becomes periodic only after a transient tail.
- You can compute powers $g^j$ by repeated squaring, but do not have inverses.
- You need to separate the preperiodic and cyclic parts before applying a group algorithm.

## Complexity

The period-finding stage is polynomial in $\log N$ with constant success probability, assuming efficient semigroup multiplication and unique encodings. The index search takes $O(\log N)$ membership tests, each checking $g^{j+r}=g^j$.

## Caveat

The trick depends on the tail being a small fraction of the sampled exponent range. It needs an upper bound $N\geq t+r$ and unique encodings so equality tests are meaningful.

## Related notes

- [[Quantum Computation of Discrete Logarithms in Semigroups (Childs-Ivanyos 2013) — Paper Notes]]
- [[Cyclic Group Extraction from a Semigroup Cycle]]
- [[Order-Finding via QFT and Continued Fractions]]
- [[Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer (Shor 1994) — Paper Notes]]
