# Variational Quantum Eigensolver (VQE)

> **Source:** Peruzzo et al., Nat. Commun. 5 (2014) [original proposal]; O'Malley, Babbush et al., arXiv:1512.06860 (2016) [first scalable hardware demo]
> **Used in:** [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
> **Tags:** #trick #VQE #variational #hybrid-classical-quantum #NISQ #quantum-chemistry

## What it does

Estimates the ground state energy of a Hamiltonian by optimizing a parameterized quantum circuit using a classical outer loop. The quantum device evaluates the energy; the classical computer updates the parameters. Designed for near-term hardware because circuit depth is controllable.

## The trick

**Setup:** Write the Hamiltonian as a sum of Pauli operators:
$$H = \sum_\gamma g_\gamma P_\gamma$$
For chemistry, this decomposition always exists and has at most $O(N^4)$ terms for $N$ orbitals.

**Variational principle:** For any state $|\phi(\vec\theta)\rangle$,
$$\frac{\langle \phi(\vec\theta)| H |\phi(\vec\theta)\rangle}{\langle\phi(\vec\theta)|\phi(\vec\theta)\rangle} \geq E_0$$

So the energy is an upper bound to the ground state energy, and minimizing it over $\vec\theta$ gives an approximation to $E_0$.

**The loop:**
1. **Prepare:** Run a parameterized circuit $U(\vec\theta)$ on the initial state $|\phi_\text{init}\rangle$:
   $$|\phi(\vec\theta)\rangle = U(\vec\theta)|\phi_\text{init}\rangle$$
2. **Measure:** Estimate each $\langle P_\gamma \rangle$ by performing appropriate basis rotations and measuring. Sum to get $E(\vec\theta) = \sum_\gamma g_\gamma \langle P_\gamma \rangle$.
3. **Optimize:** Feed $(\vec\theta, E(\vec\theta))$ to a classical optimizer (gradient descent, Nelder-Mead, COBYLA, etc.). Get new $\vec\theta'$.
4. Repeat until convergence.

**For H₂ (O'Malley et al.):** The UCCSD ansatz has one variational parameter:
$$|\phi(\theta)\rangle = e^{-i\theta X_0 Y_1}|01\rangle$$

Circuit: 11 single-qubit gates + 2 CZ$_\pi$ gates.

**Error robustness:** The classical outer loop effectively re-calibrates for hardware imperfections. If the device has systematic errors (pulse miscalibrations, crosstalk), the optimizer finds the value of $\theta$ that minimizes the energy *on the actual device*, not the ideal device. This typically gives better results than running a fixed non-variational circuit. O'Malley et al. show the experimentally optimal $\theta$ gives $14\times$ better accuracy than the theoretically optimal $\theta$ on their device.

## When to reach for it

- Ground state energy estimation without fault-tolerant quantum computing.
- Systems where you have a good ansatz that captures the relevant correlations (UCC for chemistry, hardware-efficient ansätze for generic problems).
- When you need robustness to hardware errors and can tolerate classical optimization overhead.
- As a subroutine in quantum simulation pipelines where ground state preparation is needed.

## Complexity

- **Circuit depth:** Controlled by choice of ansatz. Can be made shallow (hardware-efficient) or deeper (UCC, more expressive).
- **Measurements:** $O(N^4)$ Pauli terms for chemistry → $O(N^4)$ measurement settings. Each requires $O(1/\epsilon^2)$ shots for precision $\epsilon$ (shot noise). Total: $O(N^4/\epsilon^2)$ measurements.
- **Classical optimization:** $O(\text{parameters}^2)$ per iteration for gradient methods; number of iterations problem-dependent. For UCCSD: $O(N^4)$ parameters.
- **No fault tolerance required** if circuit is shallow enough — error rates just degrade solution quality, they don't cause catastrophic failure.

## Caveat

- **Barren plateaus:** For randomly initialized deep circuits, gradients of the energy become exponentially small in system size, making optimization intractable. Careful ansatz design or initialization is essential.
- **Local minima:** Classical optimizer can get stuck. Problem becomes harder for larger systems with many parameters.
- **Ansatz expressibility:** If the true ground state is not in the ansatz's image, VQE can't find it. A bad ansatz gives a loose upper bound, not the ground state energy.
- **Measurement overhead:** For large molecules, the $O(N^4)$ Pauli terms and required shot counts dominate runtime. Grouping commuting terms into simultaneous measurements helps, but overhead is still large.
- **Not a speedup over classical for small systems:** For H₂, exact diagonalization is trivial classically. VQE is only interesting when the molecule is large enough that the Hilbert space is classically intractable.

## Related notes
- [[Scalable Quantum Simulation of Molecular Energies (O'Malley, Babbush et al 2016) — Paper Notes]]
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Pauli Expectation Value Estimation]]
- [[Bravyi-Kitaev Transformation]]
- [[Iterative Phase Estimation (Kitaev)]]
