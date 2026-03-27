# Truncated Cluster Expansion as FPTAS

> **Source:** Helmuth, Perkins, and Regts (2020); Mann and Helmuth, arXiv:2004.11568
> **Tags:** #trick #FPTAS #cluster-expansion #approximate-counting

## What it does
Converts any convergent cluster expansion into a fully polynomial-time approximation scheme (FPTAS) by truncating the series at logarithmic order and exploiting the bounded branching of clusters on bounded-degree graphs.

## The trick
If the cluster expansion for $\log Z$ converges absolutely with $|T_m - \log Z| \leq |V| e^{-m}$, then:

1. Set $m = \lceil \log(|V|/\varepsilon) \rceil$
2. Enumerate all clusters of total size $< m$ — there are $\exp(O(m)) \cdot |V|^{O(1)}$ of them on bounded-degree graphs
3. For each cluster, compute the Ursell function ($\exp(O(m))$) and the polymer weights ($\exp(O(m))$)
4. Sum to get $T_m$, then exponentiate to get $\hat{Z}$

Since $m = O(\log(n/\varepsilon))$, all exponentials in $m$ become polynomial in $n$ and $1/\varepsilon$.

The paradigm: **convergence + efficient enumeration + efficient weight computation = FPTAS**.

## When to reach for it
- Designing classical algorithms for partition functions (quantum or classical) on bounded-degree graphs
- When you have a perturbative expansion and want to make it algorithmic
- As an alternative to Barvinok's interpolation method or Markov chain Monte Carlo

## Complexity
$|V|^{O(\log(d\Delta))} \cdot (1/\varepsilon)^{O(\log(d\Delta))}$ where $d$ is the local Hilbert space dimension and $\Delta$ is the maximum degree. Polynomial, but the degree can be large.

## Caveat
Requires the convergence condition to hold — typically $|\beta| = O(1/\Delta)$. The polynomial degree $O(\log(d\Delta))$ means this isn't practical for large $d$ or $\Delta$; it's a complexity-theoretic result, not a practical algorithm.

## Related notes
- [[Quantum Cluster Expansion for Partition Functions]]
- [[Efficient Algorithms for Approximating Quantum Partition Functions (Mann-Helmuth 2021) — Paper Notes]]
- [[Efficient Algorithms for Approximating Quantum Partition Functions at Low Temperature (Helmuth-Mann 2023) — Paper Notes]]
