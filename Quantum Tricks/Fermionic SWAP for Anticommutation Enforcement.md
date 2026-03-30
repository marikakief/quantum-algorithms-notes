# Fermionic SWAP for Anticommutation Enforcement

> **Source:** Verstraete, Cirac, Latorre, arXiv:0804.1888
> **Tags:** #trick #fermions #quantum-circuits #anticommutation #encoding

## What it does
Swaps two fermionic modes on a qubit register while correctly accounting for the $(-1)$ sign from anticommutation.

## The trick

The standard SWAP gate exchanges two qubits, but fermionic modes satisfy $\{c_i, c_j^\dagger\} = \delta_{ij}$, so swapping two occupied modes must introduce a minus sign. The fermionic SWAP gate is:

$$U_{\text{fSWAP}} = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 0 & -1 \end{pmatrix}$$

This is identical to a standard SWAP except for the $-1$ phase on $|11\rangle$ (both modes occupied). Decomposition: SWAP followed by a controlled-Z, or equivalently three CNOTs plus a phase gate.

Any circuit implementing fermionic operations on qubits via [[Jordan-Wigner Transformation for Chemistry Hamiltonians|Jordan-Wigner]] must use fermionic SWAPs whenever non-adjacent modes need to interact. The [[Fermionic Swap Network]] is the systematic application of this idea.

## When to reach for it

- Implementing fermionic Fourier transforms ([[Fermionic Fast Fourier Transform (FFFT)]]) on qubit hardware
- Building [[Fermionic Swap Network|fermionic swap networks]] for chemistry simulation
- Any circuit where Jordan-Wigner encoded fermions need reordering
- Compiling Bogoliubov transformations or Givens rotations on qubit registers

## Complexity

One fermionic SWAP = one standard SWAP + one CZ = $O(1)$ two-qubit gates. The overhead per swap is a single additional phase gate compared to a bosonic/qubit SWAP.

## Caveat

The $O(n^2)$ fermionic SWAP overhead in the [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes|Verstraete-Cirac-Latorre]] construction dominates the gate count. Alternative encodings (Bravyi-Kitaev, ternary tree) reduce the locality of fermionic operators and can reduce SWAP overhead, but at the cost of more complex gate decompositions.

## Related notes
- [[Quantum Circuits for Strongly Correlated Quantum Systems (Verstraete-Cirac-Latorre 2009) — Paper Notes]]
- [[Fermionic Fast Fourier Transform (FFFT)]]
- [[Fermionic Swap Network]]
- [[Jordan-Wigner Transformation for Chemistry Hamiltonians]]
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]]
