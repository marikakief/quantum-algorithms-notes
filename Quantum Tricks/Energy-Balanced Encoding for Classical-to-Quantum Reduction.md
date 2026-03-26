# Energy-Balanced Encoding for Classical-to-Quantum Reduction

> **Source:** Babbush, Berry, Kothari, Somma, Wiebe, arXiv:2303.13012
> **Tags:** #trick #encoding #classical-to-quantum #Hamiltonian-simulation #coupled-oscillators #energy-conservation

## What it does
Maps classical dynamics to quantum evolution with a state encoding that weights kinetic and potential energy terms equally, so the quantum state norm is the conserved total energy. This avoids condition-number-dependent bottlenecks that arise from naive encodings.

## The trick

For a classical system $\ddot{\vec{y}} = -A\vec{y}$ with $A \succeq 0$ and $A = BB^\dagger$, define the quantum state:

$$|\psi(t)\rangle = \frac{1}{\sqrt{2E}} \begin{pmatrix} \dot{\vec{y}}(t) \\ iB^\dagger \vec{y}(t) \end{pmatrix}$$

where $E = \frac{1}{2}\|\dot{\vec{y}}\|^2 + \frac{1}{2}\|B^\dagger \vec{y}\|^2$ is the total energy (kinetic + potential). This state satisfies Schrödinger's equation $|\dot{\psi}\rangle = -iH|\psi\rangle$ with the [[Incidence Matrix Factorization for Square-Root Block Encoding|block Hamiltonian]] $H$.

The norm $\langle\psi|\psi\rangle = 1$ is guaranteed by energy conservation, making the evolution unitary without any additional normalization tricks.

**Why other encodings fail:**

- $(\dot{\vec{y}},\, \vec{y})^T$ (velocity-position): $\|\vec{y}\|^2$ dominates for low-frequency modes, suppressing kinetic energy amplitudes by $O(1/\omega_k)$. Extracting kinetic energy costs $O(1/\omega_{\min})$ extra.
- $(\dot{\vec{y}},\, A\vec{y})^T$ (velocity-force): $\|\dot{\vec{y}}\|^2$ dominates, and accessing potential energy requires inverting $\sqrt{A}$ with condition-number cost.
- The energy-balanced encoding eliminates this asymmetry entirely.

## When to reach for it

- Mapping any second-order classical ODE to quantum simulation.
- When the target observable involves both position-like and momentum-like quantities.
- When the system has modes spanning a wide range of frequencies (large condition number of $A$).

## Complexity

No additional cost — the encoding is a design choice, not a computation. The encoding determines which initial states are efficiently preparable and which observables are efficiently measurable. With this encoding, kinetic and potential energy fractions are directly accessible via projective measurements on $|\psi(t)\rangle$.

## Caveat

The encoding determines what's easy to measure. Individual oscillator positions $x_j(t)$ are *not* directly encoded as amplitudes. Extracting the full configuration vector requires $O(N)$ measurements, erasing any speedup. The encoding is designed for aggregate observables (total/subset kinetic or potential energy).

## Related notes
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]]
- [[Incidence Matrix Factorization for Square-Root Block Encoding]]
- [[Square-Root Sparsity in Block Encoding]]
