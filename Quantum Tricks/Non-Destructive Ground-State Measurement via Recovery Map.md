# Non-Destructive Ground-State Measurement via Recovery Map

> **Source:** Wecker, Hastings, Wiebe, Clark, Nayak, Troyer, arXiv:1506.05135
> **Tags:** #trick #measurement #non-destructive #amplitude-estimation #quadratic-speedup

## What it does
Measures arbitrary projective observables on a ground state without destroying it, achieving $O(1/\epsilon)$ scaling instead of the standard $O(1/\epsilon^2)$ from repeated preparation and measurement.

## The trick
**Basic recovery map:** After measuring a projector $Q$ on $|\psi_0\rangle$, the state collapses to $Q|\psi_0\rangle / \|Q|\psi_0\rangle\|$. To recover $|\psi_0\rangle$:

1. Measure $P_0 = |\psi_0\rangle\langle\psi_0|$ (via coherent phase estimation — run QPE, check if the energy matches the known ground-state energy, uncompute)
2. If $P_0$ succeeds: ground state recovered. Measure $Q$ again for more statistics
3. If $P_0$ fails: measure $Q$ again, then $P_0$ again. Repeat until recovery

Expected number of recovery steps: $k$ (the number of outcomes of $Q$). This is a Markov chain; each failed recovery redistributes the state across projector eigenspaces with the original probability distribution.

**Quadratic speedup via amplitude estimation:** Define $U = 2Q - 1$ and $V = 2P_0 - 1$. The product $UV$ restricted to the 2D subspace $\text{span}\{|\psi_0\rangle, Q|\psi_0\rangle\}$ has eigenvalues $e^{\pm 2i\theta}$ where $\langle\psi_0|Q|\psi_0\rangle = \cos^2(\theta)$.

Run [[Iterative Phase Estimation (Kitaev)|phase estimation]] on $UV$ to determine $\theta$ to precision $\epsilon$. This requires $O(1/\epsilon)$ applications of $UV$ — a quadratic improvement over the $O(1/\epsilon^2)$ shots needed for direct sampling.

**Implementing $P_0$:** Use "coherent phase estimation" — run QPE on $H$, coherently check if the energy matches the ground state, flip an outcome qubit, then uncompute QPE. This implements a POVM approximating $P_0$ without revealing the energy. Alternatively, use the adiabatic map: $P_0 \approx U_{\text{adiab}} P_0^{\text{free}} U_{\text{adiab}}^\dagger$.

## When to reach for it
- Measuring multiple observables on the same ground state without re-preparing it each time
- When ground-state preparation is expensive (slow adiabatic evolution, costly QPE projection)
- Dynamic correlation functions where you want to measure at many frequencies using the same ground state

## Complexity
- Recovery map: $O(k)$ rounds of $Q$ and $P_0$ measurements per recovery, where $k$ is the number of $Q$ outcomes
- With amplitude estimation: $O(T_0 / \epsilon)$ total time, where $T_0$ is the cost of one $UV$ application
- $P_0$ implementation: requires QPE to precision $\Delta$ (the spectral gap), costing $O(1/\Delta)$ per application

## Caveat
Requires implementing $P_0$ to high fidelity, which itself requires QPE with precision better than the spectral gap $\Delta$. If $\Delta$ is exponentially small, this is impractical. Also, the quadratic speedup applies to the measurement precision, not the state preparation cost — if preparation dominates, the speedup may not help much. The coherent phase estimation for $P_0$ requires additional ancilla qubits (logarithmic in precision).

## Related notes
- [[Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) — Paper Notes]]
- [[Iterative Phase Estimation (Kitaev)]]
- [[Frequency-Domain Spectral Function via QPE Importance Sampling]]
- [[Amplitude Estimation via Phase Estimation on Q]]
