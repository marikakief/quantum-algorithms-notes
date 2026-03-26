# Fermionic Swap Network

> **Source:** Kivlichan, McClean, Wiebe, Gidney, Aspuru-Guzik, Chan, Babbush, arXiv:1711.04789 (2018)
> **Tags:** #trick #Trotter #fermionic-swap #Jordan-Wigner #linear-connectivity #circuit-compilation #quantum-chemistry

## What it does

Implements a complete first-order Trotter step of the electronic structure Hamiltonian in circuit depth exactly $N$ using $N(N-1)/2$ two-qubit entangling gates, assuming only linear nearest-neighbour qubit connectivity and the Jordan–Wigner encoding.

## The trick

The electronic structure Hamiltonian in second quantization is:

$$H = \sum_{p,q} T_{pq} a^\dagger_p a_q + \sum_p U_p n_p + \sum_{p \neq q} V_{pq} n_p n_q$$

A Trotter step requires simulating every pair interaction $(p,q)$. The challenge: under Jordan–Wigner, only adjacent orbitals give 2-local qubit operators. Non-adjacent orbital interactions require long Pauli strings.

**The fix:** change the canonical ordering dynamically using fermionic swaps. The fermionic swap between orbitals $p$ and $q$ is:

$$f_{p,q}^{\text{swap}} = 1 + a^\dagger_p a_q + a^\dagger_q a_p - a^\dagger_p a_p - a^\dagger_q a_q$$

This genuinely swaps orbital labels (not just qubit states), preserving fermionic antisymmetry. When applied to adjacent orbitals ($q = p+1$ in the current JW ordering), it is a 2-local qubit gate.

**The scheduling insight:** use odd-even transposition sort (parallel bubble sort) to permute the orbital ordering. This sort takes exactly $N$ parallel layers to reverse a length-$N$ sequence, and guarantees each of the $\binom{N}{2}$ orbital pairs becomes adjacent in exactly one layer.

**The key economy:** in the layer where pair $(p,q)$ becomes adjacent, instead of just applying the fermionic swap, apply the composite **fermionic simulation gate**:

$$F_t(p,q) = e^{-iV_{pq} n_p n_q t} \cdot e^{-iT_{pq}(a^\dagger_p a_q + a^\dagger_q a_p)t} \cdot f_{p,q}^{\text{swap}}$$

In the $\{|00\rangle, |01\rangle, |10\rangle, |11\rangle\}$ basis (for adjacent JW sites):

$$F_t = \begin{pmatrix} 1 & 0 & 0 & 0 \\ 0 & -i\sin(T_{pq}t) & \cos(T_{pq}t) & 0 \\ 0 & \cos(T_{pq}t) & -i\sin(T_{pq}t) & 0 \\ 0 & 0 & 0 & -e^{-iV_{pq}t} \end{pmatrix}$$

Each $F_t$ compiles to at most 3 entangling gates (CNOT/CZ) plus single-qubit rotations. After $N$ layers of these gates (alternating odd-even patterns), all $N(N-1)/2$ interaction pairs have been simulated, completing the first-order Trotter step. One additional layer of single-qubit $Z$-rotations handles the diagonal $U_p n_p$ terms.

**Symmetric (second-order) Trotter:** Double the final interaction strength to $2t$ and omit its fermionic swap, then mirror the remaining gates in reverse. Achieves symmetric Trotter with no additional gates.

## When to reach for it

- Compiling Trotterized time evolution for electronic structure on any linearly-connected quantum hardware (superconducting qubits, trapped ions in a chain).
- As the state-evolution subroutine in [[Iterative Phase Estimation (Kitaev)]] for quantum chemistry.
- As the Hamiltonian simulation layer in VQE circuits when the ansatz includes Trotterized evolution.
- Whenever you want to minimise two-qubit gate count for a Trotter step of a dense (all-to-all) quadratic+quartic fermionic Hamiltonian.
- The Hubbard model variant gives $O(\sqrt{N})$ depth on a linear chain — use it for 2D lattice simulation.

## Complexity

- **Depth:** exactly $N$ layers
- **Two-qubit entangling gates:** exactly $N(N-1)/2$ (one per orbital pair)
- **Primitive CNOT/CZ per Trotter step:** $\leq 3 \cdot N(N-1)/2$
- **Single-qubit rotations:** $O(N^2)$ (one per orbital pair, plus $N$ diagonal terms)
- **Hubbard-2D variant:** $O(\sqrt{N})$ depth on a linear chain

For comparison: naive JW decomposition takes $O(N^4)$ Pauli terms × $O(N)$ gates each = $O(N^5)$ gates, $O(N^5)$ depth.

## Caveat

- **JW-specific:** The trick relies on changing the canonical Jordan–Wigner ordering via fermionic swaps. It doesn't extend directly to Bravyi-Kitaev or other encodings.
- **Optimality unproved:** The paper conjectures $N(N-1)/2$ is optimal for a general-Hamiltonian Trotter step, but no formal lower bound exists.
- **No Trotter error analysis for this ordering:** The commutator-based Trotter error depends on the specific ordering of terms. The "reverse-sort" schedule may or may not be optimal for error — numerical study needed per system.
- **Periodic boundary conditions:** The Hubbard-model $O(\sqrt{N})$ result doesn't handle periodic boundary conditions.

## Related notes
- [[Quantum Simulation of Electronic Structure with Linear Depth and Connectivity (Kivlichan, McClean et al 2018) — Paper Notes]]
- [[Improved Fault-Tolerant Quantum Simulation of Condensed-Phase Correlated Electrons via Trotterization (Kivlichan, Gidney, Babbush et al 2020) — Paper Notes]] — uses this network as one of two Trotter step circuits; measures its Trotter error norm against the split-operator competitor and finds the swap network is $2$–$3\times$ worse for jellium
- [[Low-Depth Quantum Simulation of Materials (Babbush, Wiebe, McClean et al 2018) — Paper Notes]] — competing low-depth approach using FFFT-based split-operator step on a planar lattice; targets periodic systems rather than arbitrary Gaussian-basis Hamiltonians
- [[Trotterized Time Evolution for Chemistry]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[Bravyi-Kitaev Transformation]]
- [[Variational Quantum Eigensolver (VQE)]]
- [[Iterative Phase Estimation (Kitaev)]]
