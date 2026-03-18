# Coherent Metropolis Accept-Reject via Phase Estimation

> **Source:** Temme, Osborne, Vollbrecht, Poulin, Verstraete, Nature 471, 87 (2011)
> **Tags:** #trick #Metropolis #phase-estimation #Gibbs-state #thermal-state

## What it does
Implements the Metropolis accept/reject step quantumly: coherently computes energy differences via phase estimation and applies a controlled rotation encoding the Boltzmann acceptance probability, collapsing to a single-bit accept/reject outcome.

## The trick

**Setup:** Start with eigenstate $|\psi_i\rangle$ at energy $E_i$. Apply random unitary $C$ to propose a move: $C|\psi_i\rangle = \sum_k x_k^i |\psi_k\rangle$.

**Coherent phase estimation:** Don't measure the energy — just entangle it with an ancilla:
$$\sum_k x_k^i |\psi_k\rangle|E_i\rangle|E_k\rangle|0\rangle$$

**Metropolis rotation:** Apply a controlled single-qubit rotation $W(E_k, E_i)$ on the last qubit:
$$W|0\rangle = \sqrt{\min(1, e^{-\beta(E_k-E_i)})}|1\rangle + \sqrt{1 - \min(1, e^{-\beta(E_k-E_i)})}|0\rangle$$

Now the amplitude on $|1\rangle$ for each branch is $x_k^i \sqrt{f_k^i}$ where $f_k^i$ is exactly the classical Metropolis acceptance probability.

**Single-bit measurement:** Measure just the last qubit.
- $|1\rangle$: accept. Then measure energy register to collapse to a specific eigenstate.
- $|0\rangle$: reject. Use [[Marriott-Watrous Iterative Rejection for Quantum Metropolis|iterative rejection]] to recover $|\psi_i\rangle$.

The key insight: by revealing only one bit (accept/reject) rather than the full energy, the measurement disturbs the state minimally — just enough to decide, not enough to destroy.

## When to reach for it
- Preparing Gibbs states of quantum Hamiltonians
- Any quantum MCMC scheme needing an accept/reject mechanism
- Quantum simulated annealing
- When you need to implement a classical stochastic transition rule in superposition

## Complexity
One phase estimation circuit + one controlled rotation + one single-qubit measurement per step. Phase estimation dominates: $O(\text{poly}(n))$ for $n$-qubit system.

## Caveat
Requires the ability to perform coherent phase estimation (i.e., Hamiltonian simulation for various times). Phase estimation errors propagate to the acceptance probabilities — finite precision $r$ introduces $O(\beta \cdot 2^{-r})$ error in the fixed point. The symmetry condition $d\mu(C) = d\mu(C^\dagger)$ on the proposal distribution is needed for detailed balance.

## Related notes
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]]
- [[Marriott-Watrous Iterative Rejection for Quantum Metropolis]]
- [[Quantum Measurements and the Abelian Stabilizer Problem (Kitaev 1995) — Paper Notes]]
