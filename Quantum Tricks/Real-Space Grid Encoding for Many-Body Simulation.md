# Real-Space Grid Encoding for Many-Body Simulation

> **Source:** Kivlichan, Wiebe, Babbush, Aspuru-Guzik, arXiv:1608.05696 (2017); building on Wiesner (1996), Zalka (1998), Kassal et al. (2008)
> **Tags:** #trick #first-quantized #real-space #grid-encoding #position-basis #many-body

## What it does

Encodes the positions of $\eta$ quantum particles on a uniform spatial mesh, requiring $\eta D \lceil \log b \rceil$ qubits (where $b$ is grid points per spatial direction and $D$ is spatial dimension). Provides a first-quantized alternative to second-quantized orbital representations, with qubit count growing only with the number of particles and the required resolution rather than the number of orbitals.

## The trick

Divide the $D$-dimensional simulation box $[0, L]^D$ into $b$ bins per dimension, each of side length $h = L/b$. Represent particle $i$'s position by $D$ registers $|x_{i,1}\rangle \cdots |x_{i,D}\rangle$, each of $\lceil \log b \rceil$ qubits indexing the bin.

The full $\eta$-particle quantum state occupies a register:

$$|x\rangle = |x_{1,1}\rangle \cdots |x_{1,D}\rangle \, |x_{2,1}\rangle \cdots |x_{\eta,D}\rangle$$

with $\eta D \lceil \log b \rceil$ total qubits. The wavefunction $\psi(y(x))$ is stored as amplitudes over computational basis states, where $y(x)$ maps each point to the centroid of its bin.

**Relation to Hilbert space dimension:** the full Hilbert space has dimension $b^{\eta D}$. For $\eta = 2$ electrons, $D = 3$, $b = 2^{32}$ (32 bits per coordinate): $192$ qubits — already large. The CI approach of [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes|Babbush et al. (2018)]] needs only $\eta \log N$ qubits using $N$ orbital basis functions, making orbital methods more qubit-efficient for chemistry.

**Operators in this basis:**
- The potential $V(x)$ is diagonal — matrix elements computable by a potential oracle query
- The kinetic energy $T = -\sum_{i,n} \nabla_{i,n}^2 / (2m_i)$ requires a finite-difference approximation (see [[High-Order Finite Difference Kinetic Energy]])
- Time evolution can be implemented via the [[Truncated Taylor Series Simulation]] after decomposing $H$ as an LCU of adders and signature matrices

## When to reach for it

- When you want to simulate interacting particles without choosing a basis set; the grid is a natural choice for systems without clear orbital structure (ultracold gases, nuclear physics, condensed matter in a box)
- When the number of particles $\eta$ is small but they interact via a complex potential
- For systems where position-space locality matters (e.g., contact interactions, lattice models, simple models in computational physics)
- As a starting point for real-space LCU simulation; combine with [[High-Order Finite Difference Kinetic Energy]] and [[Regularized Coulomb Potential (Delta Cutoff)]]

## Complexity

- **Qubits:** $\eta D \lceil \log b \rceil$ for positions; plus ancillas for simulation ($O(\log(\eta D a + V_\text{max}/\delta))$ for the LCU index register)
- **Grid resolution required (fixed h case):** simulation cost scales as $O(h^{-2})$ from the kinetic energy 1-norm
- **Worst-case resolution (Theorem 4):** to simulate the continuous system to error $\varepsilon$, may need $h \sim (k_\text{max} L / \pi)^{-\eta D/2}$, giving exponential qubit count in $\eta D$
- **Under Corollary 5 assumptions (plane-wave-like states):** $h$ is polynomial in $1/\varepsilon$; total oracle queries $\tilde{O}(\eta^7 t^3/\varepsilon^2)$

## Caveat

- The discretization error analysis of [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes|Kivlichan et al. (2017)]] shows that, in the worst case, the grid resolution must be exponentially fine in $\eta D$. This can negate the computational advantage over second-quantized methods.
- Does not naturally accommodate systems with cusps or singularities in the wave function (e.g., electrons near nuclei). The [[Regularized Coulomb Potential (Delta Cutoff)]] provides a partial fix but introduces an additional approximation.
- Not a fermion-aware encoding: the antisymmetry of the wavefunction must be imposed separately (e.g., via initial-state preparation or by restricting to antisymmetric subspace). Compare with the CI representation, which handles antisymmetry through the Slater determinant basis.

## Related notes
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]]
- [[High-Order Finite Difference Kinetic Energy]]
- [[Regularized Coulomb Potential (Delta Cutoff)]]
- [[Controlled Swap Network for Select Oracle]]
- [[Truncated Taylor Series Simulation]]
- [[Exponentially More Precise Quantum Simulation of Fermions in the CI Representation (Babbush et al 2018) — Paper Notes]] — orbital-basis first-quantized alternative
