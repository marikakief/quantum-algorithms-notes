# Reversible Multi-Pass Streaming Predicate for Amplitude Estimation

> **Source:** Montanaro, arXiv:1505.00113
> **Tags:** #trick #streaming #amplitude-estimation #reversible-computation #space-efficient

## What it does

Makes an existential predicate over a stream coherent without storing per-item garbage.

## The trick

Suppose the predicate is

$$
f(j)=[\exists i:h_j(a_i)=1].
$$

A naive reversible implementation would store a garbage bit for every stream item. Instead:

1. First pass: compute the count
   $$
   N_j=|\{i:h_j(a_i)=1\}|.
   $$
2. Copy only the predicate bit $[N_j\ne0]$ to an output flag.
3. Second pass: subtract the same contributions and uncompute $N_j$.

This implements

$$
|j\rangle|z\rangle\mapsto |j\rangle|z+[\exists i:h_j(a_i)=1]\rangle
$$

using only $O(\log n)$ counter space beyond the hash-function workspace.

## When to reach for it

- Streaming algorithms where [[Quantum Amplitude Amplification and Estimation (Brassard-Høyer-Mosca-Tapp 2002) — Paper Notes|amplitude estimation]] should estimate the probability of a stream predicate.
- Existential predicates that can be reduced to a small reversible counter.
- Space-limited quantum streaming models where garbage accumulation would kill the advantage.

## Complexity

Each coherent predicate call costs two passes over the stream. Amplitude estimation to additive error $\epsilon$ uses $O(1/\epsilon)$ calls, hence $O(1/\epsilon)$ passes up to constants.

## Caveat

The method saves space by spending passes. It is not a one-pass streaming trick.

## Related notes

- [[Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016) — Paper Notes]]
- [[Amplitude Estimation via Phase Estimation on Q]]
- [[Reversible Computation via Compute-Copy-Uncompute]]
