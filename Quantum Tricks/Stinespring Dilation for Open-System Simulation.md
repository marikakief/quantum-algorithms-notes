
> **Source:** Lloyd, Science 273(5278), 1073–1078 (1996); Kraus, *States, Effects, and Operations* (1983)
> **Tags:** #trick #open-systems #hamiltonian-simulation #dilation

## What it does
Converts the simulation of an open quantum system (described by a CPTP map / superscattering operator) into the simulation of a closed system on a larger Hilbert space.

## The trick

Any trace-preserving completely positive map $\mathcal{S}(t)$ acting on an $m$-dimensional density matrix can be written as:

$$\mathcal{S}(t)[\rho_S(0)] = \mathrm{tr}_E\left[U(t) \left(\rho_S(0) \otimes \rho_E(0)\right) U^\dagger(t)\right]$$

for some unitary $U(t)$ on an $m^2$-dimensional system+environment Hilbert space $\mathcal{H}_S \otimes \mathcal{H}_E$, and some initial environment state $\rho_E(0)$.

The map $\mathcal{S}(t)$ has $m^4 - m^2$ free parameters. Finding $U(t)$ and $\rho_E(0)$ requires solving an $m^2 \times m^2$ matrix equation. Once found, simulate $U(t)$ using [[Product-Formula Time-Slicing for Local Hamiltonians|product-formula time-slicing]] on the enlarged space.

For **local** open systems (where each variable interacts locally with both other variables and the environment), the simulation remains efficient: the enlarged system is still local, so the product-formula approach scales polynomially.

A practical shortcut: a single ancilla qubit can be reused to simulate the environment of each system variable in turn. For $N$ spins with Bloch-equation-type decoherence, you need $N + 1$ qubits total, not $N + N$.

## When to reach for it
- Simulating decoherence, dissipation, or thermal relaxation alongside coherent dynamics
- Modeling measurement back-action or quantum channels
- When you need the full density-matrix evolution $\rho(t)$, not just unitary dynamics
- Lindblad master equations — discretise and dilate each time step

## Complexity
- **Environment overhead:** at most $m^2$ dimensions (quadratic in local system dimension)
- **Setup cost:** solving an $m^2 \times m^2$ matrix equation to find $U(t)$ — done classically once
- **Simulation cost:** same as closed-system [[Product Formulas]], applied to the enlarged space
- For local open systems with $\ell$ system terms and $\ell_E$ environment terms: $O((\ell m^2 + \ell_E m_E^4) \cdot n \cdot t^2/\varepsilon)$

## Caveat
Efficiency requires the open-system dynamics to be a **finite-order quantum Markov process** — the environment must have finite correlation lengths in time and space. Long-range temporal correlations (non-Markovian dynamics) break the local structure needed for efficient simulation.

The $m^2$-dimensional dilation is worst-case. Many physical environments are well-approximated by much smaller ancilla spaces.

## Related notes
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]]
- [[Product-Formula Time-Slicing for Local Hamiltonians]]
- [[Entropy Pumping for State Preparation]]
- [[Density Matrix Exponentiation via Partial Swap]]
