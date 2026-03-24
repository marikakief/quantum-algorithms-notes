# Fractional-Query to Discrete-Query Reduction

> **Source:** Dominic W. Berry, Andrew M. Childs, Richard Cleve, Robin Kothari, and Rolando D. Somma, *Exponential improvement in precision for simulating sparse Hamiltonians*, arXiv:1312.1414 (STOC 2014)  
> **Links:** [arXiv](https://arxiv.org/abs/1312.1414)  
> **Tags:** #trick #query-complexity #simulation #reduction

## Idea

The Berry–Childs–Cleve–Kothari–Somma algorithm (arXiv:1312.1414) uses a continuous-time oracle model and a fractional-query model as intermediate steps, then shows these are simulable by discrete quantum queries with only $O(1)$ overhead. The key result is that a $d$-sparse Hamiltonian $H$ can be simulated for time $t$ with precision $\epsilon$ using
$$O\!\left(\tau \frac{\log(\tau/\epsilon)}{\log\log(\tau/\epsilon)}\right) \text{ queries, } \quad \tau = d^2\|H\|_{\max}t.$$

## Why it matters

The continuous/fractional-query model allows a clean OAA-based analysis. The reduction shows that model is not much more powerful than discrete queries even for small error — which makes the polylog(1/ε) bound achievable in the discrete model. Without this bridge, the argument would be limited to the continuous-time setting.

## When to use it

When the analysis naturally lives in a continuous-time or fractional-query model, but the final complexity statement needs to be in standard query complexity. The reduction carries the polylog precision advantage across from the idealized model to the real one.

## Caveat

The reduction carries $O(1)$ constant overhead, which is fine for asymptotic arguments but matters for concrete implementations. Berry et al. (1412.4687) later simplified the approach by directly using the truncated [[Taylor Series Truncation with ln2 Segmentation|Taylor series]], bypassing the fractional-query intermediate step.

## Related notes

- [[Exponential Improvement in Precision for Simulating Sparse Hamiltonians (Berry-Childs-Cleve-Kothari-Somma 2014) — Paper Notes]] — the source paper (STOC 2014)
- [[Exponential Improvement in Precision for Hamiltonian-Evolution Simulation (Berry-Cleve-Somma 2013) — Paper Notes]] — initial announcement
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
