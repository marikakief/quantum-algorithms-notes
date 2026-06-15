# PGM Bootstrapping for Wildcard Search

> **Source:** Ambainis and Montanaro, arXiv:1210.1148
> **Tags:** #trick #state-discrimination #pretty-good-measurement #query-complexity #oracle-learning

## What it does

Learns an unknown bit string from wildcard subset-equality queries by growing a coherent known subset and using a pretty-good measurement to guess the newly added bits.

## The trick

Define subset states

$$
|\psi_x^k\rangle=\binom nk^{-1/2}\sum_{S\subseteq[n], |S|=k}|S\rangle|x_S\rangle.
$$

For $k=n-O(\sqrt n)$, the states $|\psi_x^k\rangle$ are distinguishable well enough that the pretty-good measurement outputs $\tilde x$ with

$$
\mathbb{E}[d(x,\tilde x)]=O(1).
$$

The Gram matrix has entries

$$
G_{xy}=\frac{\binom{n-d(x,y)}{k}}{\binom nk},
$$

so it depends only on $x\oplus y$ and diagonalises under the Fourier transform over $\mathbb{Z}_2^n$.

Use this as a bootstrapping step. Choose

$$
n_{i-1}=\lceil n_i-\sqrt{n_i}\rceil,
$$

starting from $n_0=O(\sqrt n)$ and ending at $n_\ell=n$. At each stage, apply the PGM to guess the enlarged subset, verify with one wildcard query, and correct the $O(1)$ expected errors by binary search.

## When to reach for it

- Oracle-identification models where a query can verify a whole candidate restriction.
- Settings with coherent subset states and an almost-complete reconstruction measurement.
- Problems where wrong coordinates can be found cheaply once a global guess is available.

## Complexity

Initial preparation costs $O(\sqrt n)$ queries. There are $O(\sqrt n)$ stages. Each stage uses $O(\log n)$ expected queries for verification/correction, so the total expected query count is

$$
O(\sqrt n\log n).
$$

## Caveat

The measurement is the heavy part conceptually: the query result is clean, but gate complexity for implementing the PGM is not the paper's focus. The method also needs a verification query that checks a whole subset assignment.

## Related notes

- [[Quantum Algorithms for Search with Wildcards and Combinatorial Group Testing (Ambainis-Montanaro 2012) — Paper Notes]]
- [[Pretty-Good Measurement for Wildcard Subset States]]
- [[Neighbour-Edge Adversary for Wildcard Queries]]
- [[Quantum Complexity Theory (Bernstein-Vazirani 1993) — Paper Notes]]
