# Vectorization Trick for Lindblad Simulation

## What it does

Maps a mixed-state (density matrix) evolution problem on an $N$-dimensional Hilbert space $\mathcal{H}$ to a pure-state evolution problem on $\mathcal{H} \otimes \mathcal{H}$, i.e., on $\mathbb{C}^{N^2}$. This converts the Lindblad master equation — a superoperator equation — into a standard Schrödinger-like equation that quantum simulation algorithms can act on directly.

## The trick

Write the density matrix $\rho \in \mathcal{B}(\mathcal{H})$ as a vector $|\rho\rangle\rangle \in \mathcal{H} \otimes \mathcal{H}$ via the column-stacking (or row-stacking) map:

$$\rho = \sum_{i,j} \rho_{ij} |i\rangle\langle j| \quad \longrightarrow \quad |\rho\rangle\rangle = \sum_{i,j} \rho_{ij} |i\rangle \otimes |j\rangle$$

Under this map, operator actions on $\rho$ become tensor product actions on $|\rho\rangle\rangle$:

$$A\rho B \quad \longrightarrow \quad (A \otimes B^T) |\rho\rangle\rangle$$

The Lindblad equation $\dot{\rho} = \mathcal{L}(\rho)$ then becomes $\dot{|\rho\rangle\rangle} = \hat{\mathcal{L}} |\rho\rangle\rangle$ where the vectorized Lindbladian is:

$$\hat{\mathcal{L}} = -i(H \otimes I - I \otimes H^T) + \sum_k \left(L_k \otimes \bar{L}_k - \frac{1}{2} L_k^\dagger L_k \otimes I - \frac{1}{2} I \otimes L_k^T \bar{L}_k\right)$$

Now $e^{\hat{\mathcal{L}}t}$ acts on $|\rho\rangle\rangle$ exactly as the Lindblad channel acts on $\rho$. The key: $\hat{\mathcal{L}}$ is a matrix on $\mathbb{C}^{N^2}$, and if $H$ and $\{L_k\}$ are sparse, so is $\hat{\mathcal{L}}$ — enabling sparse simulation algorithms. This is the starting point of [[Efficient Simulation of Sparse Markovian Quantum Dynamics (Childs-Li 2017) — Paper Notes|Childs-Li 2017]].

On a quantum computer, the register size doubles from $\log_2 N$ qubits (for the Hilbert space) to $2\log_2 N$ qubits (for the doubled space). Expectation values $\text{Tr}[O\rho]$ become inner products $\langle\langle O | \rho\rangle\rangle$ in the doubled space.

## When to reach for it

- Simulating open quantum system dynamics (Lindblad, GKSL master equation) on a quantum computer.
- Computing expectation values $\text{Tr}[O e^{\mathcal{L}t}(\rho_0)]$ for arbitrary sparse $H$, $L_k$.
- Reducing an open-system simulation to a (non-unitary) matrix exponentiation problem, then applying any Hamiltonian simulation or non-unitary dynamics algorithm.
- Analyzing spectral properties of the Lindbladian (eigenvalues correspond to decay rates).

## Complexity

Vectorization costs an exact factor of $N^2$ in Hilbert space dimension, i.e., 1 extra qubit register of the same size. The Lindbladian $\hat{\mathcal{L}}$ on $\mathbb{C}^{N^2}$ is $d^2$-sparse when $H$ and $L_k$ are $d$-sparse. So simulation of $e^{\hat{\mathcal{L}}t}$ using sparse Hamiltonian simulation inherits the sparse complexity with $N \to N^2$ and $d \to d^2$.

Full Lindblad simulation cost (from [[Efficient Simulation of Sparse Markovian Quantum Dynamics (Childs-Li 2017) — Paper Notes|Childs-Li 2017]]): $\tilde{O}(d^2 K \Lambda_\mathcal{L}^{1/2} t^{1/2} / \epsilon^{1/2})$ queries.

## Caveat

- The vectorized state $|\rho\rangle\rangle$ represents a density matrix only if it is positive semidefinite; numerical errors in the simulation can produce unphysical outputs that are not valid density matrices.
- $e^{\hat{\mathcal{L}}t}$ is contractive (trace-preserving) but not unitary; directly simulating it requires the Stinespring dilation or another non-unitary dynamics method.
- The doubled space costs an extra $n$ qubits for an $n$-qubit system, which doubles the qubit overhead.
- Vectorization does not commute with partial traces; care is needed when simulating subsystems or computing reduced density matrices.

## Related notes

- [[Efficient Simulation of Sparse Markovian Quantum Dynamics (Childs-Li 2017) — Paper Notes]] — source paper
- [[Stinespring Dilation for Open-System Simulation]] — complementary trick (dilating to a unitary)
- [[Single-Ancilla Lindbladian Dilation for Dissipative Simulation]] — alternative that avoids full doubling
- [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes]] — open systems as a computational resource
- [[Hamiltonian simulation]] — parent concept hub
