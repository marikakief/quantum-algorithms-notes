# Feynman Clock Lindbladian for Dissipative Computation

> **Source:** Verstraete, Wolf, Cirac, arXiv:0803.1447
> **Tags:** #trick #dissipation #Lindbladian #history-state #quantum-computation

## What it does
Encodes the output of any quantum circuit as the unique steady state of a time-independent, local Lindbladian. No unitary gates or state initialisation needed at runtime — the dissipative process drives the system to the answer.

## The trick
Given a circuit $\{U_t\}_{t=1}^T$ on $N$ qubits with history states $|\psi_t\rangle = U_t \cdots U_1 |0\rangle^{\otimes N}$, add an auxiliary clock register $\{|t\rangle\}_{t=0}^T$ and define Lindblad operators:

$$L_i = |0\rangle_i\langle 1| \otimes |0\rangle_t\langle 0| \quad (i = 1,\ldots,N)$$

$$L_t = U_t \otimes |t+1\rangle\langle t| + U_t^\dagger \otimes |t\rangle\langle t+1|$$

The $L_i$ reset the qubits to $|0\rangle$ when the clock reads $t = 0$, implementing state preparation. The $L_t$ implement bidirectional transitions between consecutive time steps, effectively running the circuit forward and backward.

The unique steady state is $\rho_0 = \frac{1}{T+1}\sum_t |\psi_t\rangle\langle\psi_t| \otimes |t\rangle\langle t|$ — a uniform mixture over the computation history. Measuring the clock register and postselecting on $|T\rangle$ gives $|\psi_T\rangle$.

**Key insight:** A similarity transformation $W = \sum_t U_t \cdots U_1 \otimes |t\rangle\langle t|$ maps the Liouvillian to one where all gates are replaced by identities. The spectrum is therefore independent of the computation — the gap is $\Delta \geq \pi^2/(2T+3)^2$ universally.

## When to reach for it
- Theoretical: proving that dissipative processes are computationally universal
- Designing self-correcting quantum memories or computation schemes
- Any setting where you want computation that naturally recovers from perturbations (the steady state is an attractor)
- The connection to adiabatic quantum computation: DQC is like AQC but with built-in relaxation instead of requiring careful gap engineering

## Complexity
Convergence time: $O(T^2)$ (quadratic in circuit depth). Success probability per measurement: $1/(T+1)$. With Kitaev's unary clock encoding, all Lindblad operators are strictly local.

## Caveat
The $1/(T+1)$ readout probability means $O(T)$ repetitions to extract the answer, giving $O(T^3)$ total time — a polynomial overhead over the circuit. For NP-verification circuits with constrained inputs, the block-diagonal structure breaks down and the gap can become exponentially small.

## Related notes
- [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Simulating Physics with Computers (Feynman 1982) — Paper Notes]]
- [[Quantum NP — Local Hamiltonian is QMA-Complete (Kitaev 1999) — Paper Notes]]
