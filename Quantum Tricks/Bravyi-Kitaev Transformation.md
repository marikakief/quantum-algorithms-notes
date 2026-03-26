# Bravyi-Kitaev Transformation

> **Source:** Bravyi & Kitaev, Ann. Phys. 298, 210 (2002); Seeley, Richard & Love, JCP 137, 224109 (2012); Tranter et al., Int. J. Quant. Chem. 115, 1431 (2015)
> **Used in:** [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
> **Tags:** #trick #fermion-mapping #qubit-encoding #quantum-chemistry

## What it does

Maps a second-quantized fermionic Hamiltonian to a qubit Hamiltonian where each qubit operator acts on at most $O(\log N)$ qubits, compared to $O(N)$ for the Jordan-Wigner transformation.

## The trick

Fermionic operators $a^\dagger_j, a_j$ satisfy canonical anti-commutation relations. To simulate them on qubits, we need a mapping that preserves these relations. The standard options are:

**Jordan-Wigner (JW):**
$$a^\dagger_j \mapsto \frac{1}{2}(X_j - iY_j) \otimes \bigotimes_{k<j} Z_k$$

Every operator $a^\dagger_j$ has a Jordan-Wigner string of $j$ Pauli-$Z$ operators. For a system with $N$ orbitals, two-body terms in the Hamiltonian become $O(N)$-local qubit operators. Gate count to simulate a single Hamiltonian term scales linearly with system size.

**Bravyi-Kitaev (BK):**

Rather than encoding occupation numbers directly (JW), BK stores a mix of occupation numbers and parities in a hierarchical binary-tree structure. Each qubit encodes:
- **Occupation** of one orbital (direct occupation)
- **Parity** of a set of orbitals (at a higher level of the tree)

The result: fermionic operators map to qubit operators with at most $\lceil \log_2 N \rceil + 1$ tensor factors. For large $N$, this is exponentially fewer Pauli factors per term.

For $\text{H}_2$ in the STO-6G basis (4 spin-orbitals → 4 qubits), the BK transform gives:

$$H = f_0 \mathbf{1} + f_1 Z_0 + f_2 Z_1 + f_3 Z_2 + f_1 Z_0 Z_1 + f_4 Z_0 Z_2 + f_5 Z_1 Z_3 + f_6 X_0 Z_1 X_2 + f_6 Y_0 Z_1 Y_2 + \ldots$$

(15 terms total for the full 4-qubit H₂ Hamiltonian, but symmetry reduction brings it to 6 terms on 2 qubits; see [[Symmetry Reduction in Qubit Hamiltonians]].)

## When to reach for it

- Simulating molecular Hamiltonians with more than a few orbitals, where JW's $O(N)$-locality becomes expensive.
- When circuit depth is the bottleneck and reducing Pauli weight matters.
- Any time you're doing VQE, QPE, or LCU-based simulation of chemistry and want scalable qubit representations.

## Complexity

- **Qubit count:** $N$ qubits for $N$ spin-orbitals (same as JW).
- **Operator locality:** $O(\log N)$ vs. $O(N)$ for JW.
- **Gate cost per Hamiltonian term:** $O(\log N)$ CX/CZ gates per term vs. $O(N)$.
- **Number of Hamiltonian terms:** $O(N^4)$ (same as JW — comes from the two-body integrals).
- **Classical preprocessing cost:** $O(N^4)$ to compute all coefficients; efficient.

## Caveat

The BK mapping is conceptually more complex than JW and the circuit for a given Hamiltonian term is harder to write by hand. For small systems (2–4 qubits) the advantage over JW is marginal. The savings only become significant at larger $N$. Also, the BK encoding breaks particle-number conservation symmetry more than JW does, complicating state preparation if you want to start from a particle-number eigenstate.

## Related notes
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]] — first hardware use of BK mapping for VQE; reduces 4-qubit operator to 2-qubit via BK + symmetry
- [[Strategies for Quantum Computing Molecular Energies Using the UCC Ansatz (Romero, Babbush et al 2018) — Paper Notes]] — shows BK reduces UCC gate cost per parameter from $O(N)$ to $O(\log N)$ vs Jordan-Wigner
- [[Symmetry Reduction in Qubit Hamiltonians]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Trotterized Time Evolution for Chemistry]]
