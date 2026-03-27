
> **Source:** Anurag Anshu, arXiv:2603.15495
> **Tags:** #trick #ground-state-preparation #local-Hamiltonian #frustration-free #heuristic #energy-lowering

## What it does

Constructs a family of Hamiltonians that all share the same low-energy subspace as the original but disagree at high energies. Switching between members of this family during energy minimisation prevents getting stuck in eigenstates of any single Hamiltonian.

## The trick

Given a local Hamiltonian $H = \sum_{i=1}^m \Pi_i$ on $n$ qubits (each $\Pi_i$ a projector acting on $k$ qubits), define the **altered Hamiltonian family**:

$$H_{\vec{\phi}} = \sum_{i=1}^m (\Pi_i + \phi_i)$$

where $\phi_i$ is a state satisfying $\Pi_i \phi_i = \phi_i \Pi_i = \phi_i$ (so $\phi_i$ lies inside the range of $\Pi_i$). The distribution $D$ over $\vec{\phi}$ is pairwise independent with each pair $(\phi_i, \phi_j)$ forming a 2-design over their respective subspaces.

**Why this works:** If $H$ has a zero-energy ground space, then every $H_{\vec{\phi}}$ shares that same ground space (adding $\phi_i$ inside $\Pi_i$'s range doesn't change the zero-energy sector). But at higher energies, different members of the family disagree — a state that is an eigenstate of $H$ is unlikely to be an eigenstate of a randomly drawn $H_{\vec{\phi}}$. The [[Energy-Dependent Uncertainty Principle for Local Hamiltonians]] formalises this: the expected variance of any state $\psi$ with respect to $H_{\vec{\phi}}$ is $\Omega(\mathrm{Tr}(H\psi))$.

**Key algebraic identity:** Since $\phi_i$ is a rank-1 projector within $\Pi_i$'s range:
$$(\Pi_i + \phi_i)^2 = \Pi_i + 3\phi_i$$

This, combined with 2-design moment averages $\mathbb{E}[\phi_i] = \Pi_i/d_i$ (where $d_i = \mathrm{rank}(\Pi_i)$), drives the variance calculation.

**The alternating algorithm:** Run any energy-lowering step (phase estimation, variational gradient descent). When progress stalls — i.e., the state is near an eigenstate of the current Hamiltonian — sample a new $H_{\vec{\phi}}$ from the family and continue. The ground space is shared, so the low-energy target is unchanged.

A key efficiency note: the distribution $D$ can be supported on $O(n)$ arrays (from standard hash function theory), so sampling a new altered Hamiltonian is computationally cheap.

## When to reach for it

- Energy minimisation is stuck in a high-energy eigenstate or local minimum
- The Hamiltonian has relatively low frustration (frustration-free or mildly frustrated)
- You have access to the local projector structure of $H$
- The ground space is zero-energy (or can be shifted to zero)

Works with both measurement-based and variational energy-lowering methods — see [[An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) — Paper Notes|Anshu (2026)]] for both variants.

## Complexity

Constructing $H_{\vec{\phi}}$: $O(n)$ samples from the 2-design distribution over each subspace. No overhead beyond preparing the random perturbation.

The overall alternating minimisation costs: $K^L$ copies for the measurement variant (Algorithm 3 in the paper, where $K$ copies per step, $L$ steps), or $O(L)$ circuit depth for the variational variant (Algorithm 4). There are no convergence guarantees — runtime is unbounded in the worst case.

## Caveat

- **No convergence proof.** The algorithm could in principle have energy oscillate up and down due to Hamiltonian switches. The paper acknowledges this explicitly.
- **Low frustration required.** For highly frustrated models, the ground space is not preserved across altered Hamiltonians in the same clean sense, and numerics show significantly worse performance.
- **Not useful for non-local Hamiltonians.** The construction requires the projector decomposition $H = \sum_i \Pi_i$. For sparse non-local Hamiltonians, the [[Sparse Altered Hamiltonians via Gaussian Perturbation]] construction is needed instead.
- The family doesn't help if you need provable bounds — this is a heuristic technique.

## Related notes
- [[An Alternating-Minimization Method for Preparing Low-Energy States (Anshu 2026) — Paper Notes]]
- [[Energy-Dependent Uncertainty Principle for Local Hamiltonians]]
- [[Sparse Altered Hamiltonians via Gaussian Perturbation]]
- [[Near-Optimal Ground State Preparation (Lin-Tong 2020) — Paper Notes]] — provably efficient alternative (requires initial overlap + gap)
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]] — dissipative approach to same problem
- [[Alternating Operator Ansatz]] — QAOA uses alternating operators; this uses alternating Hamiltonians (different level)
- [[Frustration-Free Gap Amplification via Walk Operator]] — complementary technique for frustration-free systems
