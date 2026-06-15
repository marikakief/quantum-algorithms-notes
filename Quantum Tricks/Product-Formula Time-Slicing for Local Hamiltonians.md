
> **Source:** Lloyd, Science 273(5278), 1073–1078 (1996)
> **Tags:** #trick #hamiltonian-simulation #product-formula #trotter

## What it does
Simulates the time evolution $e^{iHt}$ of a local Hamiltonian by splitting it into a product of local unitaries, each acting on a small subsystem.

## The trick

Given $H = \sum_{i=1}^{\ell} H_i$ where each $H_i$ acts on at most $k$ variables, approximate:

$$e^{iHt} \approx \left(e^{iH_1 t/n} e^{iH_2 t/n} \cdots e^{iH_\ell t/n}\right)^n$$

The error is controlled by the commutators $[H_i, H_j]$:

$$
\left\|\left(e^{iH_1 t/n}\cdots e^{iH_\ell t/n}\right)^n-e^{iHt}\right\|
= O\!\left(\frac{t^2}{n}\sum_{i>j}\|[H_i,H_j]\|\right)+O(t^3/n^2).
$$

To achieve accuracy $\varepsilon$, take

$$
n = O\!\left(\frac{t^2}{\varepsilon}\sum_{i>j}\|[H_i,H_j]\|\right)
$$

steps. In the worst case this can be upper bounded by a crude $\ell^2 h^2 t^2/\varepsilon$ expression if $\|H_i\|\le h$, but the commutator sum is the quantity the analysis is actually tracking. Each local unitary $e^{iH_i t/n}$ acts on an $m_i$-dimensional space and compiles into $O(m_i^2)$ elementary gates.

The key insight: even though $e^{iHt}$ acts on an exponentially large Hilbert space, each factor $e^{iH_i t/n}$ acts on a *constant*-dimensional local space when $H$ is local. You never need to touch the full $2^N$-dimensional space.

## When to reach for it
- Simulating any Hamiltonian that decomposes into a sum of local terms (lattice models, molecular Hamiltonians, spin chains)
- When you want a simple, intuitive simulation method before reaching for heavier machinery like [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes|QSP]] or [[LCU Origins (Childs-Wiebe 2012) — Paper Notes|LCU]]
- When the commutators $[H_i, H_j]$ are small or structured — the actual error can be much better than the worst-case bound

## Complexity
- **Trotter steps:** $n = O(t^2\sum_{i>j}\|[H_i,H_j]\|/\varepsilon)$ for first-order Trotter
- **Local exponentials:** $\ell n$
- **Gate count:** $O(m^2\ell n)$ when each local exponential is compiled by a dense $m$-dimensional synthesis routine
- **Qubits:** $O(N)$ — proportional to the number of system variables
- **Wall-clock time:** proportional to $t$ only in Lloyd's physical analog/parallel-control setting; as a circuit algorithm the relevant resources are gate count, depth, oracle queries, and synthesis precision

Higher-order Suzuki formulas improve the $\varepsilon$-dependence to $O(t^{1+1/2k}/\varepsilon^{1/2k})$ for order $2k$. See [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs et al. (2019)]] for tight bounds using nested commutators.

## Caveat
First-order Trotter scaling is $O(t^2/\varepsilon)$ in the step count for generic noncommuting decompositions — far from the optimal normalized $O(\alpha t + \log(1/\varepsilon)/\log\log(1/\varepsilon))$ achieved by [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|qubitization]] from an $\alpha$-block-encoding. For high-precision applications, [[Product Formulas]]s are outclassed asymptotically. But for moderate precision and structured Hamiltonians, the constants can be very favorable (the commutator structure often helps enormously).

## Related notes
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes]]
- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Faster Quantum Simulation by Randomization (Childs-Ostrander-Su 2019) — Paper Notes]]
