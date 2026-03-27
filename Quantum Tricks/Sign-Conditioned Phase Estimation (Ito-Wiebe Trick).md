> **Source:** Reiher, Wiebe, Svore, Wecker, Troyer, arXiv:1605.03590 (Lemma 2; attributed to unpublished work by Tsuyoshi Ito, 2012)
> **Tags:** #trick #phase-estimation #controlled-unitary #T-depth-reduction #quantum-simulation

## What it does
Halves the number of unitary applications needed in iterative [[Quantum Phase Estimation|phase estimation]] by replacing the standard controlled-$U$ with a sign-conditioned operation that applies $U$ or $U^\dagger$ depending on the control qubit state.

## The trick
Instead of implementing $\Lambda(U^M)$ (controlled-$U$ repeated $M$ times), implement $W^M$ where:

$$W|0\rangle|\psi\rangle = |0\rangle U|\psi\rangle, \qquad W|1\rangle|\psi\rangle = |1\rangle U^\dagger|\psi\rangle$$

If $U|\phi\rangle = e^{i\phi}|\phi\rangle$, then applying $H \otimes \mathbb{1}$ before and after $W^M$ on $|0\rangle|\phi\rangle$ gives measurement probability:

$$P(0|\phi;\theta,M) = \frac{1 + \cos(2M[\phi + \theta])}{2}$$

The key: the phase accumulates as $2M\phi$ rather than $M\phi$ — double the sensitivity per application.

For [[Product Formula (Trotter-Suzuki)|Trotter-Suzuki]] circuits where $U = \prod_j B_j R_z(\phi_j) B_j^\dagger$, the $W$ operation is implemented by replacing each controlled rotation with:

$$B_j \cdot \text{CNOT} \cdot (\mathbb{1} \otimes R_z(-\phi_j)) \cdot \text{CNOT} \cdot B_j^\dagger$$

using the identity $X R_z(\theta) X = R_z(-\theta)$. This requires only two CNOT gates per rotation instead of full controlled-$U$ decomposition.

## When to reach for it
- Any iterative phase estimation protocol where $U$ is decomposed into rotations (Trotter-based simulation, any rotation-heavy circuit)
- Most effective when the controlled-$U$ decomposition would otherwise be expensive
- Works whenever $U^\dagger$ can be obtained cheaply by sign-flipping rotation angles — holds for all odd-order Trotter-Suzuki formulas and for 2nd-order when the Hamiltonian is real-valued

## Complexity
Reduces the number of calls to $U$ from $M(\varepsilon) \approx \pi/\varepsilon$ to $M(\varepsilon) \approx \pi/(2\varepsilon)$. Combined with RFPE (Bayesian phase estimation), the expected cost is $M(\varepsilon) \approx 2.3/\varepsilon$.

## Caveat
Requires that $U^\dagger$ can be formed by negating all rotation angles. This is exact for 3rd-order and higher [[Product Formula (Trotter-Suzuki)|Trotter-Suzuki]] formulas, but for the 2nd-order formula it introduces $O(t^3)$ error (the Strang splitting is not exactly its own inverse when the terms don't commute). The paper argues this is acceptable for real-valued Hamiltonians targeting ground states.

For [[Qubitization (Quantum Walk for Spectral Encoding)|qubitization]]-based approaches, a different controlled-walk construction is used and this trick doesn't directly apply (qubitization already has efficient controlled walks).

## Related notes
- [[Elucidating Reaction Mechanisms on Quantum Computers (Reiher, Wiebe, Svore, Wecker, Troyer 2017) — Paper Notes]]
- [[Quantum Phase Estimation]]
- [[Programmable Ancilla Rotations (PAR) for T-Depth Reduction]]
- [[Error Budget Optimization for Fault-Tolerant Algorithms]]
