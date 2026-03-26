# Energy-Dependent Uncertainty Principle for Local Hamiltonians

> **Source:** Anurag Anshu, arXiv:2603.15495
> **Tags:** #trick #local-Hamiltonian #uncertainty-principle #variance #frustration-free #complexity

## What it does

Shows that any high-energy state of a local Hamiltonian $H$ has high variance with respect to a randomly drawn member of the [[Altered Hamiltonian Family for Escaping Eigenstates|altered Hamiltonian family]]. The variance scales at least linearly in the energy of the state.

## The trick

Given $H = \sum_{i=1}^m \Pi_i$ (sum of projectors, each acting on $k$ qubits), and the altered Hamiltonian family $\{H_{\vec{\phi}}\}$ drawn from a pairwise-independent 2-design distribution $D$:

**Theorem 2.1 (Anshu 2026):** For any quantum state $\psi$:
$$\mathbb{E}_{\vec{\phi} \sim D}\, \mathrm{Var}_\psi(H_{\vec{\phi}}) = \Omega(1) \cdot \mathrm{Tr}(H\psi) = \Omega(1) \sum_i \mathrm{Tr}(\Pi_i \psi)$$

where $\mathrm{Var}_\psi(G) = \mathrm{Tr}(G^2 \psi) - \mathrm{Tr}(G\psi)^2$.

The same bound holds relative to any fixed altered Hamiltonian $H_{\vec{\phi}'}$:
$$\mathbb{E}_{\vec{\phi} \sim D}\, \mathrm{Var}_\psi(H_{\vec{\phi}}) = \Omega(1) \cdot \mathrm{Tr}(H_{\vec{\phi}'} \psi)$$

**Proof sketch.** Expand $\mathrm{Var}_\psi(H_{\vec{\phi}})$ and take expectation over $D$. The 2-design averages are:
$$\mathbb{E}[\phi_i] = \frac{\Pi_i}{d_i}, \quad \mathbb{E}[\phi_i \otimes \phi_i] = \frac{\Pi_i^{\otimes 2}(I + \mathrm{Swap})\Pi_i^{\otimes 2}}{d_i(d_i+1)}$$
Using the key identity $(\Pi_i + \phi_i)^2 = \Pi_i + 3\phi_i$ and the purity bound $\mathrm{Tr}((\Pi_i \psi_i \Pi_i)^2) \leq \mathrm{Tr}(\Pi_i \psi_i)^2$, one derives:
$$\mathbb{E}\, \mathrm{Var}_\psi(H_{\vec{\phi}}) \geq \mathrm{Var}_\psi\!\left(\sum_i (1 + 1/d_i)\Pi_i\right) + \frac{1}{\max_i d_i} \sum_i \mathrm{Tr}(\Pi_i \psi)$$

The first term is non-negative, the second term is the claimed linear lower bound.

**Interpretation.** A zero-energy state has zero variance with any $H_{\vec{\phi}}$ (it's in the kernel of every $\Pi_i + \phi_i$). A high-energy state must have high variance — it cannot be close to an eigenstate of a typical $H_{\vec{\phi}}$. The uncertainty principle says: high energy ↔ high spectral spread across the altered Hamiltonian.

**Tightness.** The linear bound is tight: product states have $\Theta(n)$ energy and $\Theta(n)$ variance with local Hamiltonians. You cannot improve to super-linear variance for the local family without additional structure.

## When to reach for it

- Proving that an energy-lowering algorithm can make progress from a high-energy stuck state
- Designing alternating-minimisation methods that exploit Hamiltonian switching
- Any setting where you need to show a state cannot be simultaneously low-variance with respect to multiple different Hamiltonians

For the analogous bound with **quadratic** energy dependence (much stronger for non-zero energies), use [[Sparse Altered Hamiltonians via Gaussian Perturbation]] instead — but that requires classical (diagonal) Hamiltonians.

## Complexity

No algorithmic cost. This is a mathematical statement about expected variance. The implied sampling cost is $O(n)$ random variables drawn from a 2-design.

## Caveat

- **Expected variance ≠ actual variance.** The bound is on $\mathbb{E}[\mathrm{Var}_\psi(H_{\vec{\phi}})]$, not on $\mathrm{Var}_\psi(H_{\vec{\phi}})$ for a specific draw. Concentration (i.e., most Hamiltonians having high variance) would require bounding $\mathbb{E}[(\mathrm{Var} - \mathbb{E}[\mathrm{Var}])^2]$, which involves 4th moments and is an open problem.
- **Variance ≠ spectral weight below mean.** What energy-lowering algorithms actually need is that a significant fraction of spectral weight of $\psi$ lies below the current mean energy (the "quartile gap"). High variance is necessary but not sufficient. This connection is supported numerically but not proved.
- Linear vs quadratic: the linear bound $\Omega(E)$ is weaker than desired. For product states it's tight, so no improvement is possible for the local family alone.

## Related notes
- [[An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) — Paper Notes]]
- [[Altered Hamiltonian Family for Escaping Eigenstates]]
- [[Sparse Altered Hamiltonians via Gaussian Perturbation]] — stronger (quadratic) version for sparse/classical Hamiltonians
- [[Clique Bound from Uncertainty Principle]] — another uncertainty principle with algorithmic consequences
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]] — uses eigenstate filtering rather than uncertainty principles to reach the ground state
