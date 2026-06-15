# Streaming Maximum Finding via Frequency Oracles

> **Source:** Montanaro, arXiv:1505.00113; Dürr-Høyer, quant-ph/9607014
> **Tags:** #trick #streaming #maximum-finding #frequency-moments #Grover

## What it does

Computes the maximum frequency $F_\infty=\max_j n_j$ in a stream by using passes as coherent frequency queries.

## The trick

For a candidate value $j$, one pass over the stream can implement

$$
|j\rangle|x\rangle\mapsto |j\rangle|x+n_j\rangle
$$

by incrementing the counter whenever $a_i=j$. A second pass uncomputes the counter when needed.

This gives coherent oracle access to the list

$$
(n_1,n_2,\ldots,n_m),
$$

so [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes|Dürr--Høyer maximum finding]] returns $\max_j n_j$ with $O(\sqrt m)$ oracle calls. After hashing the universe to $O(n)$ effective size, this is $O(\sqrt n)$ passes.

## When to reach for it

- Streaming problems where each item defines an implicit score for each label.
- Small-memory settings where the score for a candidate label can be recomputed by scanning the stream.
- Exact maximum/minimum tasks over an implicit array.

## Complexity

For $F_\infty$, Montanaro gives $O(\log^2 m)$ qubits and $O(\sqrt n)$ passes, assuming the effective universe is at most polynomial in $n$.

## Caveat

This is exact but pass-heavy. It is attractive only when passes are cheap and memory is the constrained resource.

## Related notes

- [[Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016) — Paper Notes]]
- [[A Quantum Algorithm for Finding the Minimum (Dürr-Høyer 1996) — Paper Notes]]
- [[Controlled Grover Iteration for Frequency Extraction]]
