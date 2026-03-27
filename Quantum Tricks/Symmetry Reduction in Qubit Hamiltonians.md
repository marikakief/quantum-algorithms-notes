
> **Source:** O'Malley, Babbush et al., arXiv:1512.06860 (2016)
> **Used in:** [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
> **Tags:** #trick #symmetry #qubit-reduction #quantum-chemistry #Hamiltonian

## What it does

Reduces the number of active qubits in a molecular simulation by identifying qubits that are stabilized (never changed) by the Hamiltonian starting from a fixed initial state. Each frozen qubit halves the Hilbert space and removes one qubit from the circuit.

## The trick

When simulating a molecular Hamiltonian, you typically start in the Hartree-Fock state $|\phi_\text{HF}\rangle$, a classical bitstring. After applying the [[Bravyi-Kitaev Transformation]], some qubits only appear in diagonal (commuting) terms of the Hamiltonian — they are never flipped by any off-diagonal term.

**Concretely for H₂ (STO-6G, BK encoding):**

The full BK Hamiltonian acts on 4 qubits. Off-diagonal terms only involve qubits 0 and 2 (those with $X$ and $Y$ factors). Qubits 1 and 3 appear only in diagonal terms ($Z_1$, $Z_3$, etc.) and are stabilized throughout the evolution. Since the Hartree-Fock initial state is a classical bitstring, the values of qubits 1 and 3 are known throughout the simulation.

Replace each stabilized qubit's contribution by its classical expectation value. The 4-qubit 15-term Hamiltonian reduces to:

$$\tilde{H} = g_0 \mathbf{1} + g_1 Z_0 + g_2 Z_1 + g_3 Z_0 Z_1 + g_4 X_0 X_1 + g_5 Y_0 Y_1$$

(2 active qubits, 6 terms).

**General procedure:**
1. Apply your fermion-to-qubit mapping.
2. Identify qubits that are eigenvalues of all off-diagonal terms starting from your initial state (equivalently: qubits that commute with the full Hamiltonian and are stabilized by the initial state).
3. Substitute their classical values and simplify. Each eliminated qubit saves one qubit and reduces gate count.
4. Note that the term $Z_0 Z_1$ further commutes with all remaining terms — its contribution can be computed directly from the initial state without any quantum circuit.

## When to reach for it

- Any chemistry simulation starting from a Hartree-Fock or other product-state reference.
- Symmetry sectors (total particle number, total spin, spatial symmetries) all give frozen qubits.
- After applying any qubit mapping (JW, BK, or others) — always check for stabilized qubits before finalizing your circuit.
- Good habit: run this check after every fermion→qubit transformation to get the minimal active space.

## Complexity

- **Qubit savings:** $k$ frozen qubits → $k$ fewer qubits, $2^k$ smaller Hilbert space.
- **Term savings:** proportional to number of terms involving frozen qubit indices.
- **Classical cost:** $O(N^4)$ to identify which terms commute with what; this is part of normal preprocessing.

## Caveat

Only works when the initial state is a stabilizer eigenstate of the frozen qubits — i.e., you're starting in a known classical bitstring (like Hartree-Fock). If your initial state is a superposition over multiple particle-number sectors, or you're doing a fully coherent simulation without a fixed starting state, there may be fewer or no frozen qubits. Also, this approach is specific to the chosen fermion-to-qubit mapping; a different mapping will produce different symmetry structure.

## Related notes
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
- [[Bravyi-Kitaev Transformation]]
- [[Variational Quantum Eigensolver (VQE)]]
