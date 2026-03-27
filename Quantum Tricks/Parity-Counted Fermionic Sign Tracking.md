
> **Source:** Abrams & Lloyd, arXiv:quant-ph/9703054 (1997)
> **Tags:** #trick #fermions #second-quantization #jordan-wigner

## What it does
Determines the correct fermionic sign for hopping between two sites in a second-quantized simulation by counting the parity of occupied states between them.

## The trick

In second quantization, the creation and annihilation operators $c_i^\dagger, c_j$ anticommute. When a fermion hops from site $j$ to site $i$, the operator $c_i^\dagger c_j$ picks up a sign $(-1)^{p}$ where $p$ is the number of occupied modes between $i$ and $j$ in the ordering.

To implement this on a quantum computer:
1. Loop over all qubits between positions $i$ and $j$
2. Count the number of occupied states (qubits in $|1\rangle$)
3. Set a flag qubit to the parity of this count
4. Perform the hopping operation (a $2 \times 2$ rotation in the subspace of sites $i, j$) with the sign conditioned on the flag
5. Uncompute the flag

This is the operational content of the [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Jordan-Wigner transformation]] — the string of $Z$ operators between sites $i$ and $j$ is exactly this parity count.

## When to reach for it
- Any second-quantized fermionic simulation using the Jordan-Wigner mapping
- Implementing hopping terms in lattice models (Hubbard, Fermi-Hubbard, chemistry Hamiltonians)
- Understanding why Jordan-Wigner strings have $O(m)$ cost per term

## Complexity
$O(m)$ gates per hopping term (one CNOT per intervening qubit for the parity count). This gives $O(m^2)$ total for a Hamiltonian with $O(m)$ hopping terms — the bottleneck in second-quantized simulation.

## Caveat
The $O(m)$ cost per term is the main disadvantage of Jordan-Wigner versus other fermion-to-qubit mappings (Bravyi-Kitaev reduces this to $O(\log m)$). For large systems, the parity strings dominate the gate count.

## Related notes
- [[Simulation of Many-Body Fermi Systems on a Universal Quantum Computer (Abrams-Lloyd 1997) — Paper Notes]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
- [[SWAP-UP Routing for Fermionic Sign Operations]]
