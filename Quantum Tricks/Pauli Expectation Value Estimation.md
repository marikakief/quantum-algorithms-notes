
> **Source:** McClean et al., NJP 18, 23023 (2016); O'Malley, Babbush et al., arXiv:1512.06860 (2016)
> **Used in:** [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
> **Tags:** #trick #measurement #Pauli #VQE #expectation-value

## What it does

Estimates the expectation value of a many-qubit Hamiltonian $H = \sum_\gamma g_\gamma P_\gamma$ by measuring each Pauli term $P_\gamma$ separately via basis rotation, then summing the weighted results. Total measurement cost scales polynomially in the number of Hamiltonian terms.

## The trick

Any qubit Hamiltonian can be written as a sum of tensor products of Pauli operators (Pauli strings):

$$H = \sum_\gamma g_\gamma P_\gamma, \quad P_\gamma \in \{I, X, Y, Z\}^{\otimes n}$$

For chemistry, the decomposition has $O(N^4)$ terms for $N$ orbitals.

To measure $\langle P_\gamma \rangle = \langle \psi | P_\gamma | \psi \rangle$:

1. **Basis rotation:** Each qubit measured in a non-$Z$ basis needs a rotation. For a Pauli string $P_\gamma = P^{(0)} \otimes P^{(1)} \otimes \ldots$:
   - $P^{(j)} = X$: apply $Y_{\pi/2}$ rotation before measuring qubit $j$ in the $Z$ basis.
   - $P^{(j)} = Y$: apply $X_{\pi/2}$ rotation before measuring qubit $j$ in the $Z$ basis.
   - $P^{(j)} = Z$ or $I$: no rotation needed.
2. **Measure** all qubits in the $Z$ basis.
3. **Compute parity** of the relevant qubits: the expectation value equals the expected parity (product of $\pm 1$ outcomes) for the non-identity qubits.
4. **Repeat** $O(1/\epsilon^2)$ times to estimate $\langle P_\gamma \rangle$ to precision $\epsilon$ (shot noise).
5. **Sum:** $\langle H \rangle = \sum_\gamma g_\gamma \langle P_\gamma \rangle$.

Each Pauli term requires a separate set of circuit runs (separate measurement basis). Total shots: $O(N^4 / \epsilon^2)$ for a molecular Hamiltonian with $N$ orbitals.

**Grouping optimization:** Pauli strings that mutually commute can be measured simultaneously (in the same basis). Partitioning $\{P_\gamma\}$ into commuting groups reduces the number of distinct circuit runs. For chemistry Hamiltonians, this can reduce measurement settings by $O(N)$ in practice.

In O'Malley et al., the H₂ Hamiltonian has 5 non-trivial terms. Each is measured with partial tomography (appropriate basis rotations) and summed according to Eq. (4) of the paper.

## When to reach for it

- Estimating the energy of any state via VQE or energy verification.
- Any time you need $\langle O \rangle$ for an operator that can be written as a Pauli sum.
- As a building block in quantum algorithms that require expectation value feedback (QAOA, variational quantum simulation, quantum machine learning).

## Complexity

- **Distinct measurement settings:** Up to $O(N^4)$ for a molecular Hamiltonian; reduced by commuting group partitioning.
- **Shots per setting:** $O(1/\epsilon^2)$ for additive precision $\epsilon$.
- **Total shots:** $O(N^4/\epsilon^2)$ in the worst case; $O(N^3/\epsilon^2)$ with good grouping.
- **Gate overhead per setting:** $O(n)$ single-qubit rotation gates for $n$-qubit Pauli string.

## Caveat

- Shot noise is unavoidable: each Pauli expectation value is a binomial estimator with variance $\leq 1$. For chemical accuracy, you need $\epsilon \sim 10^{-3}$ Hartree, which requires $\sim 10^6$ shots per Pauli term. With $O(N^4)$ terms for larger molecules, this is the dominant cost.
- Readout errors (misassignment of $|0\rangle/|1\rangle$) corrupt the parity estimates. O'Malley et al. correct for this by measuring a calibration matrix and inverting.
- Does not detect or correct errors that change qubit state mid-circuit. Works best when readout is the dominant error source.

## Related notes
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Unitary Coupled Cluster (UCC) Ansatz]]
