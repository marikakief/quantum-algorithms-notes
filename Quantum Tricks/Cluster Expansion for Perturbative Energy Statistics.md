# Cluster Expansion for Perturbative Energy Statistics

> **Source:** Altshuler, Krovi, Roland, arXiv:0912.0746
> **Tags:** #trick #perturbation-theory #statistical-mechanics #random-instances

## What it does
Shows that perturbative energy corrections for localised states on random instances decompose into sums of independent random terms, giving controlled statistics for energy differences.

## The trick
Consider a state $|\sigma\rangle$ localised at a vertex of a high-dimensional graph. The $m$-th order perturbative energy correction from hopping with amplitude $\lambda$ can be expanded via the cluster expansion (borrowed from statistical mechanics):

$$E^{(m)}(\sigma) = \sum_{\text{clusters } C \text{ of size } m} \lambda^{2m} \, g_C(\sigma)$$

where each cluster $C$ is a connected subgraph of the hypercube rooted at $\sigma$. For random instances with $M = \alpha N$ clauses, the key observation: different clusters are (approximately) independent random variables with $O(1)$ variance.

The energy *difference* between two solutions $\sigma_1, \sigma_2$ at order $m$ then satisfies:

$$|E^{(m)}_1 - E^{(m)}_2|^2 = O(N)$$

because there are $O(N)$ independent clusters contributing, each with $O(1)$ variance. By the central limit theorem, the total splitting at any given $\lambda$ is $O(\sqrt{N})$, and the expected number of anti-crossings between two solutions grows with $N$.

## When to reach for it
- Estimating energy splittings between localised states in random Hamiltonians
- Showing that two solutions whose cost-function values are "accidentally" close will generically have an anti-crossing in the adiabatic path
- Any perturbative analysis on random combinatorial instances where you need to control the statistics of corrections rather than their worst-case values

## Complexity
The expansion is perturbative (requires $\lambda < \lambda_{\text{cr}}$). Computing individual cluster contributions is polynomial in $m$, but the analysis is statistical — you track distributions, not individual values.

## Caveat
The independence of cluster contributions is approximate and relies on the random-instance structure (clauses drawn uniformly). For structured or planted instances, clusters can be correlated and the $\sqrt{N}$ scaling breaks down. The expansion also truncates at finite order, and resumming the series is non-trivial.

## Related notes
- [[Anderson Localization Makes Adiabatic Quantum Optimization Fail (Altshuler-Krovi-Roland 2010) — Paper Notes]]
- [[Anderson Localisation on the Hypercube as AQO Obstruction]]
