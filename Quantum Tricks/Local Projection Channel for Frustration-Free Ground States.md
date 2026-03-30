# Local Projection Channel for Frustration-Free Ground States

> **Source:** Verstraete, Wolf, Cirac, arXiv:0803.1447
> **Tags:** #trick #dissipation #frustration-free #state-engineering #ground-state

## What it does
Drives any quantum state toward the ground space of a frustration-free Hamiltonian by randomly measuring local projectors and applying corrections, with each step monotonically increasing the ground-state fidelity.

## The trick
For a frustration-free Hamiltonian $H = \sum_\lambda H_\lambda$ (each $H_\lambda$ a local projector, with $P_\lambda = \mathbf{1} - H_\lambda$ projecting onto the local ground space), define the completely positive map:

$$T(\rho) = \sum_\lambda p_\lambda \left[ P_\lambda \rho P_\lambda + \frac{1}{m}\sum_{i=1}^{m} U_{\lambda,i} H_\lambda \rho H_\lambda U_{\lambda,i}^\dagger \right]$$

The first term keeps the part of $\rho$ already in the local ground space. The second term takes the part in the excited space, applies a correction unitary $U_{\lambda,i}$ to rotate it back toward the ground space, and depolarises over $m$ choices of correction.

The monotonicity $\text{tr}[T(\rho)\Psi] \geq \text{tr}[\rho\Psi]$ holds because the projection step can only increase overlap with the global ground state $\Psi$ (which, by frustration-freeness, is simultaneously in the ground space of every $H_\lambda$).

**Speed depends on the Hamiltonian structure:**

- **Commuting $H_\lambda$ with local corrections:** If $[U_{\lambda,i}, H_{\lambda'}] = 0$ for $\lambda \neq \lambda'$, correcting one region doesn't disturb others. Convergence: $O(N \log N)$.
- **Graph states (stabiliser):** $U_\lambda = \sigma_z^{(\lambda)}$ gives $q = 0$ (correction always succeeds on first try), and the Lindbladian has gap exactly 1. Convergence: $O(\log N)$.
- **General frustration-free:** Convergence is $O(N^N)$ — a proof of existence, not practical.

## When to reach for it
- Preparing ground states of frustration-free Hamiltonians (stabiliser codes, topological codes, AKLT states, specific [[Projected Entangled Pairs as Variational Ansatz|PEPS]])
- Designing self-correcting quantum memories: the dissipative process continuously drives back to the code space
- Cooling protocols: the channel mimics what a physical cooling bath does, but with engineered specificity

## Complexity
Varies dramatically by Hamiltonian class: $O(\log N)$ for graph states, $O(N \log N)$ for commuting injective Hamiltonians, $O(N^N)$ worst case.

## Caveat
Only works for frustration-free Hamiltonians — the ground state must minimise every local term simultaneously. Most physically interesting Hamiltonians (e.g., the Heisenberg antiferromagnet on a frustrated lattice) are *not* frustration-free. The channel also requires knowing the local projectors $P_\lambda$ and designing appropriate correction unitaries, which may not be straightforward for complex Hamiltonians.

## Related notes
- [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes]]
- [[Projected Entangled Pairs as Variational Ansatz]]
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]]
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]]
