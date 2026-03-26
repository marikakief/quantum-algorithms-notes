# First-Quantized Plane-Wave Chemistry Encoding

> **Source:** Babbush, Berry, McClean, Neven, arXiv:1807.09802 (2019)
> **Tags:** #trick #first-quantized #plane-wave #qubit-encoding #antisymmetry #momentum-space

## What it does

Encodes an $\eta$-electron wavefunction in a plane-wave orbital basis using $O(\eta \log N)$ qubits — dramatically fewer than the $N$ qubits needed for second-quantized encodings — by storing one momentum-space index per electron rather than one qubit per orbital.

## The trick

In second quantization ($N$ qubits for $N$ orbitals), each qubit indicates whether an orbital is occupied. For $\eta$ electrons in $N$ orbitals, this wastes $N - \eta$ qubits on unoccupied states, and the Hamiltonian norm scales with $N$ as if all orbitals were filled.

In **first-quantized momentum-space encoding**, instead use $\eta$ registers, one per particle, each holding $\log N$ qubits (a 3D grid index $p \in G = [-N^{1/3}/2, N^{1/3}/2]^3 \subset \mathbb{Z}^3$). The state is:

$$|\psi\rangle = \sum_{p_\ell \in G} \alpha_{p_1 \cdots p_\eta} |p_1\rangle_1 \cdots |p_\eta\rangle_\eta$$

Antisymmetry is maintained in the wavefunction coefficients (not the operators):

$$\alpha_{p_1 \cdots p_i \cdots p_j \cdots p_\eta} = -\alpha_{p_1 \cdots p_j \cdots p_i \cdots p_\eta}$$

This means swapping any two electron registers induces a phase of $-1$. Time evolution under any fermionic Hamiltonian preserves antisymmetry (since fermionic Hamiltonians commute with the permutation operator), so once an antisymmetric initial state is prepared, it stays antisymmetric.

**Initial-state antisymmetrization** of an arbitrary product state costs $O(\eta \log\eta \log N)$ using a comparison/sorting network approach (Berry et al. 2018).

No Jordan–Wigner strings, no [[Bravyi-Kitaev Transformation]], no fermion-to-qubit mapping overhead. The qubit count is just $\eta \lceil 3\log_2 N^{1/3} \rceil = O(\eta \log N)$.

The Hamiltonian in this representation splits as $H = T + U + V$ where:
- $T = \sum_j \sum_p \|k_p\|^2/2\, |p\rangle\langle p|_j$ is diagonal in the computational basis
- $U + V$ shifts individual particle momenta by $\pm\nu$, acting as one-body ($U$) and two-body ($V$) operators on pairs of registers

This structure is critical: the **1-norm of the potential** $\lambda = O(\eta^{5/3} N^{1/3})$ is much smaller than the **kinetic norm** $\|T\| = O(N^{2/3}/\eta^{1/3})$ when $N \gg \eta$, enabling the [[Interaction Picture Simulation (Kinetic Frame)|interaction picture]] approach.

## When to reach for it

- Any simulation where $N \gg \eta$ (large basis, few electrons) — the advantage over second quantization grows as $(N/\eta)^{2/3}$ in qubit count
- When you want to exploit $\|T\| \gg \|U+V\|$ for interaction-picture simulation
- Molecular simulation in the plane-wave basis (where $N \approx 100\eta$ is typical for chemical accuracy)
- Simulations without the Born-Oppenheimer approximation (nuclei can be included as additional particles)

## Complexity

- **Qubits:** $O(\eta \log N)$ (vs. $N$ for second quantization)
- **Initial antisymmetrization:** $O(\eta \log\eta \log N)$ gates
- **Per-oracle call cost** (SELECT for $U$/$V$): $O(\eta \log N)$ gates

## Caveat

Antisymmetry is not automatic — it must be prepared correctly and maintained throughout. Any operation that doesn't commute with the permutation group will violate antisymmetry. The "register" structure also means that particle labels are explicit: SELECT oracles must iterate over all $\eta$ registers to apply one-body or two-body operations, costing $O(\eta)$ per oracle call (vs. $O(1)$ per term in second quantization).

The encoding also lacks the variational error bound that comes from Galerkin discretization in Gaussian orbital bases. For molecules, convergence with $N$ is $O(1/N)$ (same as Gaussian asymptotically, but with a larger prefactor — roughly $100\times$ more plane waves than Gaussians for chemical accuracy).

## Related notes
- [[Quantum Simulation of Chemistry with Sublinear Scaling in Basis Size (Babbush, Berry, McClean, Neven 2019) — Paper Notes]]
- [[Fault-Tolerant Quantum Simulations of Chemistry in First Quantization (Su, Berry, Wiebe, Rubin, Babbush 2021) — Paper Notes]] — compiles this encoding to explicit fault-tolerant circuits with optimised constant factors
- [[Interaction Picture Simulation (Kinetic Frame)]]
- [[Kinetic Energy as Bit-Product LCU]] — exploits the diagonal structure of $T$ in this encoding
- [[CI Matrix Simulation (First-Quantized Encoding)]] — alternative first-quantized approach in Gaussian orbitals
- [[Bounding the Costs of Quantum Simulation of Many-Body Physics in Real Space (Kivlichan, Wiebe, Babbush, Aspuru-Guzik 2017) — Paper Notes]] — first-quantized simulation in real space (position grid vs. momentum grid)
- [[Plane-Wave Dual Basis]] — second-quantized counterpart using the Fourier dual of plane waves
- [[Bravyi-Kitaev Transformation]] — the mapping this approach replaces for plane-wave simulation
- [[Quantum Simulation of Realistic Materials in First Quantization Using Non-Local Pseudopotentials (Berry, Rubin, Babbush et al 2024) — Paper Notes]] — extends this encoding to realistic materials with pseudopotentials and non-cubic cells
- [[Gramian-Based Momentum Norm for Non-Cubic Cells]] — generalises the momentum norm computation for non-orthogonal Bravais lattices
