# Symmetry-Breaking Encoding via Degenerate Ground Spaces

> **Source:** Cubitt & Montanaro, arXiv:1311.3161, SICOMP 45(2), 2016
> **Tags:** #trick #encoding #symmetry-breaking #perturbation #QMA

## What it does

Encodes a logical qubit into the degenerate ground space of a multi-qubit gadget, breaking a continuous symmetry that the physical interaction possesses. This lets you simulate interactions that the original symmetry would forbid.

## The trick

When an interaction $h$ is invariant under $U^{\otimes 2}$ for all $U$ in some group $G$ (e.g. $\mathrm{SU}(2)$ for the Heisenberg interaction), any Hamiltonian built purely from $h$ inherits this symmetry. You can't directly encode an asymmetric target Hamiltonian.

The workaround:

1. Build a small gadget (e.g. 4 qubits in a "star" graph) using only $h$-type interactions
2. Tune weights so the ground space is exactly 2-dimensional — this encodes one logical qubit
3. Within this encoded subspace, the full $G$-symmetry is broken. The encoded qubit transforms under a *representation* of $G$ that is non-trivial but restricted
4. Add weaker inter-gadget interactions to generate effective logical interactions between encoded qubits
5. The second-order [[Perturbation Gadgets for Locality Reduction|perturbative expansion]] produces the desired target interactions on the encoded qubits

For the Heisenberg model specifically: 3 physical qubits per logical qubit, arranged so the Heisenberg star Hamiltonian has a 2D ground space. The **Lieb-Mattis model** (Heisenberg on a complete bipartite graph) provides an exactly solvable case with the needed properties: unique ground state, computable correlation functions ($\langle \psi | X_i X_j | \psi \rangle = -2/n$).

## When to reach for it

- Proving QMA-completeness of symmetric Hamiltonians (Heisenberg, XY)
- Constructing [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes|universal Hamiltonians]] from symmetric building blocks
- Any simulation where the available interactions have more symmetry than the target

## Complexity

Constant overhead per logical qubit (e.g. 3:1 or 4:1 physical-to-logical). The gadget construction is efficient.

## Caveat

Requires finding an exactly solvable instance of the symmetric model with the right ground-space structure. This is model-specific and can be hard — the Heisenberg case relies on special properties of the Lieb-Mattis model. Also, the encoding adds auxiliary qubits and requires large energy penalties, which may be impractical.

## Related notes

- [[Complexity Classification of Local Hamiltonian Problems (Cubitt-Montanaro 2016) — Paper Notes]]
- [[Universal Quantum Hamiltonians (Cubitt-Montanaro-Piddock 2018) — Paper Notes]]
- [[Perturbation Gadgets for Locality Reduction]]
- [[Mediator Qubit Gadget for Interaction Generation]]
