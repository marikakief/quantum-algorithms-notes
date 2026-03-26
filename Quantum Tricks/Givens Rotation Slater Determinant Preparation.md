# Givens Rotation Slater Determinant Preparation

> **Source:** Kivlichan, McClean, Wiebe, Gidney, Aspuru-Guzik, Chan, Babbush, arXiv:1711.04789 (2018)
> **Tags:** #trick #state-preparation #Slater-determinant #Givens-rotation #linear-connectivity #initial-state #VQE

## What it does

Prepares an arbitrary single Slater determinant (the many-body fermionic state corresponding to a single-particle unitary $u \in U(N)$) in circuit depth at most $N/2$ on a linearly connected qubit chain, using $N(N-1)/2$ nearest-neighbour two-qubit Givens rotations.

## The trick

A Slater determinant for $\eta$ electrons in $N$ orbitals is specified by an $N \times N$ unitary $u$ (the transformation from the computational to the molecular orbital basis). The many-body state is $U(u)|\mathbf{f}_0\rangle$ where $|\mathbf{f}_0\rangle$ is a fixed $\eta$-electron Fock state.

The single-particle unitary $u$ can be decomposed into **Givens rotations** — two-dimensional unitary rotations that zero out one matrix element at a time (like the classical QR / Householder decomposition). Each Givens rotation $G_{pq}(\theta, \phi)$ acting on orbitals $p$ and $q$ has the single-particle form:

$$G_{pq} = \begin{pmatrix} \cos\theta & -e^{i\phi}\sin\theta \\ e^{-i\phi}\sin\theta & \cos\theta \end{pmatrix}$$

and lifts to a 2-local qubit gate when $q = p+1$ in the Jordan–Wigner ordering.

**Decomposition procedure:**
1. Decompose $u$ into $N(N-1)/2$ nearest-neighbour Givens rotations by zeroing one sub-diagonal element at a time (column by column, moving diagonally).
2. Schedule the rotations in parallel layers: rotations acting on disjoint qubit pairs can execute simultaneously.

**Parallel schedule depth reduction:**
- Naive sequential depth: $O(N^2)$
- Parallel nearest-neighbour schedule: depth $2N - 3$
- **Spin symmetry (SU(2)):** if $u$ is block-diagonal in spin-up/spin-down ($u = u_\uparrow \oplus u_\downarrow$), run both spin sectors on interleaved qubits in parallel → depth $N - 3$
- **Particle-number symmetry:** only $\eta(N - \eta)$ Givens rotations suffice (to map the $\eta$-electron reference to the target determinant). With optimal scheduling, depth $\leq \eta - 1$. For $\eta \leq N/2$ this gives depth $\leq N/2 - 1$. (If $\eta > N/2$, apply rotations on holes instead.)
- **Combined bound:** depth $\leq N/2$ for arbitrary Slater determinants.

## When to reach for it

- Preparing the Hartree-Fock initial state (or any other single-determinant reference) for VQE or phase estimation circuits.
- As the first layer of a UCC ansatz circuit: start from the Slater determinant reference, then apply correlated excitations.
- Whenever you need to prepare a fermionic Gaussian state on a linear qubit chain.
- State preparation for adiabatic ramp algorithms that begin from a mean-field reference.

## Complexity

- **Givens rotations (worst case):** $N(N-1)/2$
- **Givens rotations (with particle-number symmetry):** $\eta(N-\eta)$
- **Circuit depth:** $\leq N/2$
- **Primitive entangling gates:** $\leq 3 \cdot N(N-1)/2$ (each Givens rotation compiles to $\leq 3$ CNOTs/CZs)
- **Connectivity required:** linear nearest-neighbour only

For comparison: Wecker et al. [41] require $N^2$ rotations at arbitrary connectivity and do not exploit parallel scheduling; depth is $O(N^2)$.

## Caveat

- **Depth bound requires both symmetries:** The $\leq N/2$ depth bound uses both spin and particle-number symmetry. Without these, the parallel schedule gives depth $2N - 3$.
- **Multi-determinant states need more:** This technique prepares a single Slater determinant. Correlated states (FCI, CASSCF, UCC) require additional entangling gates on top of this state preparation layer.
- **Compilation overheads:** The Givens decomposition is computed classically. For large $N$, the decomposition itself is $O(N^3)$ floating-point operations, which is fine classically but worth noting if automated recompilation is frequent (e.g., per VQE iteration with orbital optimisation).

## Related notes
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]]
- [[Fermionic Swap Network]]
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Active Space Restriction for VQE (CAS-UCC)]]
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]]
- [[Sequential Multi-Determinant State Preparation]] — generalises single-det preparation to $L$-determinant superpositions; uses ASCI to select and weight the determinants
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — paper that extends this single-determinant technique to multi-determinant case
