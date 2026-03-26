# Plane-Wave Dual Basis

> **Source:** Babbush, Wiebe, McClean, McClain, Neven, Chan, arXiv:1706.00023 (2018)
> **Tags:** #trick #plane-wave #dual-basis #periodic-systems #second-quantized #Hamiltonian-reduction #DVR

## What it does

Reduces the number of distinct second-quantized Hamiltonian terms for a periodic electronic system from $O(N^4)$ (Gaussian basis) or $O(N^3)$ (plane-wave basis) to $\Theta(N^2)$ by choosing a basis where kinetic and potential energies are diagonal in complementary representations.

## The trick

For a periodic system (box of volume $\Omega$) with $N$ plane-wave modes $\{e^{ik \cdot r}/\sqrt{\Omega}\}_{k \in G}$, define the **dual basis** as the discrete Fourier transform of the plane-wave basis:

$$\chi_j(r) = \frac{1}{\sqrt{N}} \sum_{k \in G} e^{ik \cdot r_j} \phi_k(r), \quad j = 1, \ldots, N$$

where $\{r_j\}$ is a dual real-space grid (the collocation points). This is the discrete-variable representation (DVR) / sinc-DVR adapted to periodic boundary conditions.

**Key property:** Under the collocation approximation (treating the sampled basis as exact),

- The **potential energy** $V(\hat{r})$ is diagonal in the dual basis: $\langle \chi_j | V | \chi_{j'} \rangle \approx V(r_j) \delta_{jj'}$
- The **kinetic energy** $T = -\nabla^2/2$ is diagonal in the plane-wave basis

So the second-quantized Hamiltonian splits as:

$$H = \underbrace{\sum_{p,\sigma} \frac{k_p^2}{2} a^\dagger_{p\sigma} a_{p\sigma}}_{T: N \text{ terms, plane-wave basis}} + \underbrace{\sum_{j,\sigma} U_j n_{j\sigma} + \sum_{j < j', \sigma,\sigma'} V_{jj'} n_{j\sigma} n_{j'\sigma'}}_{U + V: \Theta(N^2) \text{ terms, dual basis}}$$

where $V_{jj'} = \frac{1}{N\Omega} \sum_{\nu \neq 0} \tilde{V}_\nu e^{i\nu \cdot (r_j - r_{j'})}$ are precomputed from the Fourier-space Coulomb coefficients $\tilde{V}_\nu = 4\pi/|\nu|^2$ (for the bare Coulomb potential).

The Jordan–Wigner encoding maps this Hamiltonian to $\Theta(N^2)$ Pauli terms, most of which are diagonal $Z$/$ZZ$-type (one-local and two-local). This structure enables:

1. All $U + V$ terms to commute → measure all simultaneously in dual basis
2. All $T$ terms to commute → measure all simultaneously after [[Fermionic Fast Fourier Transform (FFFT)]]
3. Trotter step structure: alternately evolve $T$ (in plane-wave basis) and $U+V$ (in dual basis), connected by FFFT

## When to reach for it

- Simulating periodic materials or the uniform electron gas (jellium) — natural setting for plane waves
- Any regime where reducing the Hamiltonian 1-norm $\lambda$ matters (LCU / Taylor series simulation)
- Variational algorithms on periodic systems where shot count is the bottleneck
- When hardware has planar (2D grid) qubit connectivity — the dual basis enables $O(N)$-depth Trotter steps on a planar lattice

## Complexity

Hamiltonian term count: $\Theta(N^2)$ vs. $O(N^4)$ (Gaussian) or $O(N^3)$ (bare plane-wave). The 1-norm $\lambda \in O(N^{5/3})$ at fixed charge density $\rho = \eta/\Omega$, compared to $O(N^{4/3} \log N)$ for Gaussian basis at fixed density (Babbush 2016 NJP).

Trotter step depth: $O(N)$ on a planar lattice. Total Trotter depth: $O(N^{7/2}/\sqrt{\varepsilon})$ at fixed density.

LCU (on-the-fly): total depth $\widetilde{O}(N^{8/3})$, gate count $\widetilde{O}(N^{11/3})$, at fixed density.

## Caveat

- **Periodic systems only.** The plane-wave and dual bases assume periodic boundary conditions (Born-von Kármán). Not applicable to isolated molecules without modification.
- **Collocation approximation.** The diagonality of the potential in the dual basis holds exactly only for the sampled potential $V(r_j)$, not the full continuous $V(r)$. This introduces basis-set incompleteness error scaling as $O(1/N)$ for smooth potentials — same rate as Gaussian orbitals near cusps. See Appendix E of arXiv:1706.00023.
- **Pseudopotentials needed for core electrons.** For real materials with heavy atoms, pseudopotentials must be used to eliminate core electrons; otherwise $N$ grows impractically fast.
- **Planar connectivity requirement.** Implementing all $O(N^2)$ pairwise $ZZ$ rotations in $O(N)$ depth requires a 2D qubit grid (Appendix H). On a linear chain, this layer costs $O(N^2)$ depth.

## Related notes
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]]
- [[Fermionic Fast Fourier Transform (FFFT)]]
- [[Variational Measurement Reduction via Dual Basis]]
- [[Truncated Taylor Series Simulation]]
- [[Real-Space Grid Encoding for Many-Body Simulation]]
- [[High-Order Finite Difference Kinetic Energy]]
- [[Trotterized Time Evolution for Chemistry]]
