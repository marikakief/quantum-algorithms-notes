
> **Source:** Kassal, Jordan, Love, Mohseni, Aspuru-Guzik, PNAS 105, 18681 (2008); based on Zalka (1998) and Wiesner (1996)
> **Tags:** #trick #quantum-simulation #real-space #QFT #split-operator

## What it does
Simulates the time evolution of a wavefunction on a real-space grid by alternating between position and momentum representations via the QFT, applying diagonal unitaries in each basis.

## The trick

For $\hat{H} = \hat{T} + \hat{V}$ where $\hat{T}$ is diagonal in momentum and $\hat{V}$ is diagonal in position:

$$|\psi(\delta t)\rangle \approx \text{QFT} \cdot e^{-iT(p)\delta t} \cdot \text{QFT}^\dagger \cdot e^{-iV(x)\delta t} |\psi(0)\rangle$$

1. Store the wavefunction on a grid of $2^{nd}$ points using $nd$ qubits ($n$ per dimension, $d$ dimensions)
2. Apply $e^{-iV(x)\delta t}$ in the position basis — diagonal, so each computational basis state picks up a phase
3. QFT to momentum basis
4. Apply $e^{-iT(p)\delta t}$ — also diagonal (kinetic energy is $p^2/2m$)
5. Inverse QFT back to position basis
6. Repeat

The diagonal unitaries are applied via [[Fourier-Eigenstate Kickback for Arithmetic Oracles|Fourier-eigenstate kickback]]: compute the function value into an ancilla prepared as a Fourier eigenstate of addition, getting the phase for free. The ancilla decouples and doesn't need to be uncomputed.

## When to reach for it
- Simulating particles in real space (chemical dynamics, scattering, strong-field physics)
- Problems where the Hamiltonian splits naturally into position-diagonal and momentum-diagonal parts
- When you want a grid-based approach with exponential spatial resolution per qubit

## Complexity
- Qubits: $nd + O(m)$ for $d$ dimensions at $n$-bit spatial resolution and $m$-bit potential precision
- Gates per timestep: dominated by potential evaluation (e.g. $O(B^2 m^3)$ for Coulomb) + $O(nd \log(nd))$ for QFTs
- Error: $O(\delta t^2)$ per step (first-order Trotter); improvable with higher-order decompositions

## Caveat
Only works cleanly when $\hat{H} = \hat{T}(p) + \hat{V}(x)$. Velocity-dependent potentials or magnetic fields require more care. The Trotter error means many short timesteps — for long-time dynamics the gate count can be large.

## Related notes
- [[Polynomial-Time Quantum Algorithm for the Simulation of Chemical Dynamics (Kassal-Jordan-Love-Mohseni-Aspuru-Guzik 2008) — Paper Notes]]
- [[Fourier-Eigenstate Kickback for Arithmetic Oracles]]
- [[Product-Formula Time-Slicing for Local Hamiltonians]]
- [[Universal Quantum Simulators (Lloyd 1996) — Paper Notes]]
