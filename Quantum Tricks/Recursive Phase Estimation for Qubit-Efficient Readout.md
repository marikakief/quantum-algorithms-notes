# Recursive Phase Estimation for Qubit-Efficient Readout

> **Source:** Aspuru-Guzik, Dutoi, Love, Head-Gordon, Science 309 (2005)
> **Tags:** #trick #phase-estimation #quantum-chemistry #qubit-efficiency

## What it does
Extracts a phase (energy eigenvalue) to $k$ bits of precision using only a small fixed-size readout register (e.g., 4 qubits), by iteratively refining the estimate.

## The trick

Standard [[Gapped Phase Estimation|phase estimation]] needs $k$ readout qubits for $k$ bits of precision. The recursive version:

1. Run a small (4-qubit) PEA on $U = e^{iH\tau}$ to get the first 4 bits of $\phi$
2. Construct a lower bound $\phi_0$ from this estimate
3. Define $V_1 = (e^{-i2\pi\phi_0} V_0)^2$ — shift the phase and square the operator
4. Run 4-qubit PEA on $V_1$ — get the next bit
5. Repeat: each iteration $k$ gives one more bit of $\phi$

The shift centres the remaining phase near $1/2$ on each iteration, and the squaring doubles the resolution. After $k$ iterations, you have $k$ bits of precision using only 4 readout qubits.

## When to reach for it
- Quantum chemistry eigenvalue calculations where qubits are precious
- Any phase estimation task where the readout register is the qubit bottleneck
- When you can afford deeper circuits but not wider ones

## Complexity
- **Qubits saved:** $k - 4$ readout qubits (for $k$-bit precision)
- **Circuit depth cost:** Each iteration doubles the number of controlled-$U$ applications (because of the squaring). After $k$ iterations, the last step requires $O(2^k)$ applications — same total as standard PEA, just serial instead of parallel.
- **Classical overhead:** Essentially none; just track the running estimate $\phi_k$

## Caveat
The squaring means later iterations have exponentially deeper circuits — you're trading qubit width for circuit depth. This is fine for classical simulation of quantum circuits (as in the original paper) but may be problematic on real hardware where decoherence limits circuit depth. Later variants (Bayesian/iterative PEA by Svore, Wiebe et al.) handle noise more gracefully.

## Related notes
- [[Simulated Quantum Computation of Molecular Energies (Aspuru-Guzik-Dutoi-Love-Head-Gordon 2005) — Paper Notes]]
- [[Gapped Phase Estimation]]
- [[Quantum Algorithm Providing Exponential Speed Increase for Finding Eigenvalues and Eigenvectors (Abrams-Lloyd 1999) — Paper Notes]]
