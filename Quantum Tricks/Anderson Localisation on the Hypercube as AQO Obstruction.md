# Anderson Localisation on the Hypercube as AQO Obstruction

> **Source:** Altshuler, Krovi, Roland, arXiv:0912.0746
> **Tags:** #trick #adiabatic #Anderson-localization #spectral-gap #negative-result

## What it does
Maps a combinatorial optimisation Hamiltonian to an Anderson model on a high-dimensional graph, revealing that eigenstate localisation prevents efficient adiabatic evolution.

## The trick
The standard adiabatic optimisation Hamiltonian $\hat{H}(\lambda) = \hat{H}_P + \lambda \hat{H}_0$ has exactly the structure of an Anderson tight-binding model when written in the computational basis:

$$\hat{H}(\lambda) = \sum_\sigma E_P(\sigma)|\sigma\rangle\langle\sigma| + \lambda \sum_{\sigma \sim \sigma'} |\sigma\rangle\langle\sigma'|$$

The cost function $E_P(\sigma)$ plays the role of random on-site disorder, and the transverse-field driver $\hat{H}_0$ provides nearest-neighbour hopping on the $N$-dimensional hypercube (Hamming graph on $\{0,1\}^N$).

For random constraint satisfaction instances near the satisfiability threshold, solutions are few and separated by Hamming distance $\Theta(N)$. The tunnelling matrix element between two solutions at distance $n$ enters only at $n$-th order perturbation theory, giving:

$$V_{12} < (A\lambda)^n$$

for some constant $A = O(1)$. Combined with energy splittings of order $\sqrt{N}$ (from [[Cluster Expansion for Perturbative Energy Statistics]]), this produces a minimum gap:

$$\Delta_{\min} \sim \exp\!\left[-\frac{v(\alpha)N}{8}\ln\!\left(\frac{N}{N_0}\right)\right]$$

which is super-exponentially small in $N$.

## When to reach for it
- Analysing whether an adiabatic algorithm will encounter exponentially small gaps on random instances of a combinatorial problem
- Any situation where the cost landscape has isolated minima separated by large Hamming distance on a discrete configuration space
- Identifying whether a proposed quantum optimisation scheme is secretly fighting Anderson localisation

## Complexity
The analysis is perturbative and requires the hopping parameter $\lambda < \lambda_{\text{cr}} \sim 1/\log N$. The resulting runtime lower bound is $T \sim 1/\Delta_{\min}^2$, which is super-exponential.

## Caveat
The mapping requires the configuration space to have a natural graph structure where the driver Hamiltonian is local hopping. Non-standard drivers (e.g. multi-spin flips, catalysts) break the analogy. Modified adiabatic paths (e.g. [[Quantum Search by Local Adiabatic Evolution (Roland-Cerf 2002) — Paper Notes|local adiabatic scheduling]], [[On The Power of Coherently Controlled Quantum Adiabatic Evolutions (Kieferová-Wiebe 2014) — Paper Notes|coherent control]]) could help if the gap locations were known, but for random instances they aren't.

Also, localisation on the hypercube is not rigorously established beyond the perturbative regime — the argument is physically compelling but not a mathematical theorem.

## Related notes
- [[Anderson Localization Makes Adiabatic Quantum Optimization Fail (Altshuler-Krovi-Roland 2010) — Paper Notes]]
- [[Cluster Expansion for Perturbative Energy Statistics]]
- [[How Powerful is Adiabatic Quantum Computation (van Dam-Mosca-Vazirani 2001) — Paper Notes]]
- [[Quantum Computation by Adiabatic Evolution (Farhi-Goldstone-Gutmann-Sipser 2000) — Paper Notes]]
- [[Gap-Adapted Adiabatic Scheduling]]
