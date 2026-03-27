# Quantum Cluster Expansion for Partition Functions

> **Source:** Mann and Helmuth, arXiv:2004.11568; Netočný and Redig (2004)
> **Tags:** #trick #partition-function #cluster-expansion #classical-algorithm

## What it does
Decomposes a quantum partition function $Z_G(\beta) = \mathrm{Tr}(e^{-\beta H_G})$ into a sum over compatible sets of "polymers" — connected multisets of edges — then takes the logarithm to get a convergent power series (the cluster expansion) when the coupling is weak enough.

## The trick
Define a **polymer** $\gamma$ as a multiset of edges in the interaction graph $G$ whose support is connected. Two polymers are **compatible** if their supports are vertex-disjoint. Then:

$$Z_G(\beta) = \sum_{\Gamma \in \mathcal{G}} \prod_{\gamma \in \Gamma} w_\gamma, \qquad \log Z_G(\beta) = \sum_{\Gamma \in \mathcal{G}^C} \varphi(H_\Gamma) \prod_{\gamma \in \Gamma} w_\gamma$$

where $\mathcal{G}$ is the set of admissible (compatible) polymer sets, $\mathcal{G}^C$ is the set of clusters (connected in the incompatibility graph), $\varphi$ is the Ursell function, and the polymer weight involves traces of symmetrised products of interaction operators.

The key insight: this is the quantum analogue of the Mayer cluster expansion from classical statistical mechanics. The Taylor expansion of $e^{-\beta H}$ naturally decomposes into connected components when grouped by which edges interact.

## When to reach for it
- When you need a classical algorithm for computing quantum partition functions at high temperature
- When studying computational complexity transitions for quantum statistical mechanics as a function of $\beta$
- When you want to prove zero-freeness of $Z_G(\beta)$ in a region of the complex $\beta$-plane

## Complexity
Evaluating the truncated expansion to order $m$ costs $\exp(O(m)) \cdot |V|^{O(1)}$. For multiplicative $\varepsilon$-approximation, $m = O(\log(|V|/\varepsilon))$, giving polynomial total time.

## Caveat
Convergence requires $|\beta| = O(1/\Delta)$ — the high-temperature regime. At low temperature, the expansion diverges and you need different tools (Pirogov-Sinai theory, or the stable-perturbation approach of [[Efficient Algorithms for Approximating Quantum Partition Functions at Low Temperature (Helmuth-Mann 2023) — Paper Notes|Helmuth-Mann (2023)]]).

## Related notes
- [[Efficient Algorithms for Approximating Quantum Partition Functions (Mann-Helmuth 2021) — Paper Notes]]
- [[Truncated Cluster Expansion as FPTAS]]
- [[Telescoping Product Estimation for Partition Functions]]
