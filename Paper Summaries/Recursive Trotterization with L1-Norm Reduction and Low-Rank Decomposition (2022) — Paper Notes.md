
> **Source:** Guang Hao Low, Yuan Su, Yu Tong, and Minh C. Tran, *On the complexity of implementing Trotter steps*, arXiv:2211.09133, PRX Quantum **4**, 020323 (2023)  
> **Links:** [arXiv](https://arxiv.org/abs/2211.09133) · [PRX Quantum](https://doi.org/10.1103/PRXQuantum.4.020323)  
> **Tags:** #hamiltonian-simulation #trotter #qubitization #block-encoding #power-law #low-rank

---

## What the paper does

Addresses a bottleneck that the Trotter error literature mostly ignores: even when you know a product formula will work (e.g., using the commutator bounds from 1912.08854), implementing each Trotter step naively costs $O(L)$ gates for an $L$-term Hamiltonian. For structured Hamiltonians, this can be reduced to sublinear-in-$L$.

Two main tools:
1. **Recursive block encoding**: for power-law decaying interactions, recursively group terms across distance scales to exploit the hierarchy.
2. **Average-cost simulation**: simulate an average Trotter step whose implementation cost is lower than the worst-case cost.

Both approaches overcome the normalization-factor barrier that makes advanced block-encoding techniques otherwise expensive inside product-formula subroutines.

## Main complexity results

For power-law interactions with decay exponent $\alpha$ in 1D:
- Standard Trotter step cost: $O(n^2)$ (all pairwise terms).
- With recursive decomposition: sublinear in $n$ for appropriate $\alpha$ regimes.

For Hamiltonians where blocks of coefficients have rank $\rho$:
- Step cost scales roughly as $O(\rho n\,\mathrm{polylog})$ in favorable cases.
- Near-linear spacetime scaling when $\rho$ is small.

For the uniform electron gas with $n$ spin orbitals and $\eta$ electrons in second quantization:
$$
\left(\eta^{1/3}n^{1/3} + \frac{n^{2/3}}{\eta^{2/3}}\right) n^{1+o(1)}
$$
total gates — asymptotically improving over previous work.

## Lower bound

For generic $n$-qubit 2-local Hamiltonians with commuting terms, at least $\Omega(n^2)$ gates are required to evolve with accuracy $\epsilon = \Omega(1/\mathrm{poly}(n))$ for time $t = \Omega(\epsilon)$. Proved via a gate-efficient reduction from approximate synthesis of diagonal unitaries in the Hamming weight-2 subspace. So structure is not just useful — it is *necessary* for sub-quadratic performance.

## Context in the simulation stack

The 1912.08854 theory tells you when Trotter error can be small (commutator structure). This paper tells you how expensive each Trotter step actually is to implement (gate complexity). They're complementary.

The paper also shows these methods can overcome the normalization-factor barrier of block-encoding approaches, making structured Trotter implementation genuinely competitive with qubitization-based methods in appropriate regimes.

## Reusable tricks extracted

- [[Recursive Interval Decomposition for Power-Law Hamiltonians]]
- [[Hierarchical Low-Rank Hamiltonian Decomposition]]

## References within this paper

- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs, Su, Tran, Wiebe & Zhu (2021)]] — [[Trotter Commutator-Scaling Bound|commutator-scaling]] error theory
- [[Efficient Quantum Algorithms for Simulating Sparse Hamiltonians (Berry-Ahokas-Cleve-Sanders 2005) — Paper Notes|Berry, Ahokas, Cleve & Sanders (2005)]] — original Suzuki-based simulation
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes|Low & Chuang (2019)]] — qubitization (asymptotic competitor)
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|Gilyén et al. (2019)]] — [[QSVT Meta-Template|QSVT]]
- Haah, Hastings, Kothari & Low (2021) — quantum algorithm for simulating real-time evolution (related low-rank techniques)

---

## Cross-references

- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[LCU Origins (Childs-Wiebe 2012) — Paper Notes]]
- [[Optimal Hamiltonian Simulation by QSP (Low-Chuang 2016-2017) — Paper Notes]]
- [[Hamiltonian Simulation by Qubitization (Low-Chuang 2019) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Hamiltonian Simulation — Comparison Tables]]
