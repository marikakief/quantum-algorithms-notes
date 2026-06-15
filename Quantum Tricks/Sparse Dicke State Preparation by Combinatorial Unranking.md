# Sparse Dicke State Preparation by Combinatorial Unranking

> **Source:** Khattar, Shutty, Gidney, Zalcman, Yosri, Maslov, Babbush, Jordan, arXiv:2510.10967
> **Tags:** #trick #state-preparation #Dicke-state #combinatorics #DQI

## What it does

Prepares a uniform superposition over $k$-subsets using a sorted list of selected indices, saving space when $k \ll m$.

## The trick

Instead of encoding a Dicke state as an $m$-qubit Hamming-weight string,

$$
|D^m_k\rangle =
\frac{1}{\sqrt{\binom{m}{k}}}
\sum_{y: |y|=k}|y\rangle,
$$

encode the same subset as a sorted list of indices:

$$
|SD^m_k\rangle =
\frac{1}{\sqrt{\binom{m}{k}}}
\sum_{1 \le c_1 < \cdots < c_k \le m}
|c_1\rangle\cdots |c_k\rangle .
$$

This uses $k\lceil \log_2 m\rceil$ qubits rather than $m$ qubits. Prepare the state by:

1. Preparing a uniform superposition over ranks $0,\ldots,\binom{m}{k}-1$.
2. Applying a reversible unranking map from the combinatorial number system:

$$
r = \sum_{j=1}^{k} \binom{c_j}{j}.
$$

The paper gives two unranking circuits: an asymptotic divide-and-conquer version using hypergeometric prefix sums and binary splitting, and a lower-constant iterative version using greedy binomial search.

## When to reach for it

- DQI-style error-location superpositions where the error count $\ell$ is much smaller than the code length $m$.
- Any subset superposition where later computation can scan membership sequentially.
- State preparation where qubits, not gates, are the main bottleneck.

## Complexity

The iterative version uses $2kb+b$ qubits and $O(mk+k^2b^2)$ Toffoli gates, where $b=\lceil\log_2 m\rceil$. The divide-and-conquer version gives $\widetilde{O}(m)$ gates under fast reversible arithmetic assumptions.

## Caveat

Sparse encoding shifts cost into membership tests. In the DQI circuit this is acceptable because the algorithm scans $i=1,\ldots,m$ and can use [[Unary Iteration]]-style comparison sharing. It is less useful if the algorithm needs random access to the entire dense Hamming-weight string.

## Related notes

- [[Verifiable Quantum Advantage via Optimized DQI Circuits (Khattar, Shutty, Gidney et al 2025) — Paper Notes]]
- [[Dicke State Preparation via Inequality Testing]]
- [[Sequential Syndrome Accumulation with Measurement-Based Error Uncomputation]]
- [[Unary Iteration]]
