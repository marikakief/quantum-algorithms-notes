# Unitary Coupled Cluster (UCC) Ansatz

> **Source:** Hoffmann & Simons, JCP 88, 993 (1988) [original UCC]; Bartlett, Kucharski & Noga, CPL 155, 133 (1989); Taube & Bartlett, IJQC 106, 3393 (2006); O'Malley, Babbush et al., arXiv:1512.06860 (2016) [hardware demo]
> **Used in:** [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
> **Tags:** #trick #ansatz #quantum-chemistry #VQE #coupled-cluster

## What it does

Parameterizes a many-body quantum state as $e^{T(\vec\theta) - T^\dagger(\vec\theta)}|\phi_\text{HF}\rangle$ where $T$ is an anti-Hermitian cluster operator built from fermionic excitations. The unitary exponential guarantees a normalized state and satisfies the variational principle. Believed to be classically intractable to simulate for general molecules.

## The trick

Classical coupled cluster (CC) parameterizes the ground state as $e^T |\phi_\text{HF}\rangle$ where $T$ is the cluster operator (a sum of excitation operators). The exponential is non-unitary, so CC doesn't satisfy the variational principle and the wavefunction isn't normalized naturally.

**UCC** makes it unitary by using $e^{T - T^\dagger}$:

$$|\phi(\vec\theta)\rangle = e^{T(\vec\theta) - T^\dagger(\vec\theta)} |\phi_\text{HF}\rangle$$

The cluster operator up to doubles (UCCSD):

$$T^{(1)}(\vec\theta) = \sum_{\substack{i \in \text{occ} \\ a \in \text{virt}}} \theta^a_i\, a^\dagger_a a_i$$

$$T^{(2)}(\vec\theta) = \frac{1}{4}\sum_{\substack{i_1, i_2 \in \text{occ} \\ a_1, a_2 \in \text{virt}}} \theta^{a_1 a_2}_{i_1 i_2}\, a^\dagger_{a_2} a_{i_2} a^\dagger_{a_1} a_{i_1}$$

Properties:
- **Variational:** $\langle \phi(\vec\theta)|H|\phi(\vec\theta)\rangle \geq E_0$ for all $\vec\theta$.
- **More general than CC:** UCC applies to multi-reference initial states; classical CC requires a single Slater determinant reference.
- **Strictly more expressive than CCSD** for exact solutions.

**For H₂ in the minimal basis (after [[Bravyi-Kitaev Transformation]] + [[Symmetry Reduction in Qubit Hamiltonians]]):** The UCCSD ansatz has exactly one free parameter:

$$|\phi(\theta)\rangle = e^{-i\theta X_0 Y_1}|01\rangle$$

Circuit implementation: 11 single-qubit gates + 2 CZ$_\pi$ gates. This is the entire [[Variational Quantum Eigensolver (VQE)]] ansatz circuit.

**Initial parameter guess via MP2:** For UCCSD, start with Møller-Plesset second-order perturbation theory amplitudes:
$$\theta^{ab}_{ij} = \frac{h_{ijba} - h_{ijab}}{\epsilon_i + \epsilon_j - \epsilon_a - \epsilon_b}$$
Singles vanish due to Brillouin's theorem ($\theta^a_i = 0$). For strongly correlated systems, MP2 may be a poor starting point; use geometry-interpolation between related systems instead.

## When to reach for it

- As the ansatz in [[Variational Quantum Eigensolver (VQE)]] for molecular ground state energies.
- When you need a variational method that is strictly more powerful than classical CCSD.
- When your system has strong multi-reference character (multiple near-degenerate configurations) where classical CC fails but UCC may still work.
- When you want a chemically motivated, physically interpretable ansatz (as opposed to a hardware-efficient but opaque one).

## Complexity

- **Parameters:** $O(N^2)$ singles + $O(N^4)$ doubles = $O(N^4)$ total for $N$ orbitals.
- **Circuit depth:** Each term in $T - T^\dagger$ requires $O(\log N)$ gates (after BK transformation). Total circuit depth for UCCSD is $O(N^4 \log N)$ — deeper than hardware-efficient ansätze.
- **Classical intractability:** No efficient classical algorithm for simulating UCC is known for general systems. This is what motivates its use on a quantum device.

## Caveat

- UCC circuits are deep — the $O(N^4)$ terms each require a non-trivial circuit block. For large $N$, this requires error correction.
- The unitary exponential $e^{T - T^\dagger}$ doesn't factorize nicely, so circuit implementations typically use the first-order Trotter approximation $\prod_k e^{\theta_k (T_k - T_k^\dagger)}$, introducing systematic error.
- Classical intractability is empirical evidence, not a proven lower bound. A polynomial-time classical simulator for UCCSD would break the computational assumption underlying the whole approach.
- Poor initialization (far from the true ground state) can cause the classical optimizer in VQE to get stuck in local minima.

## Related notes
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Bravyi-Kitaev Transformation]]
- [[Symmetry Reduction in Qubit Hamiltonians]]
