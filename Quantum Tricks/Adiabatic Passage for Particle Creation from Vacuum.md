# Adiabatic Passage for Particle Creation from Vacuum

> **Source:** Jordan, Krovi, Lee, Preskill, arXiv:1703.00454
> **Tags:** #trick #state-preparation #adiabatic-passage #quantum-field-theory #particle-creation

## What it does
Creates exactly one particle in a potential well starting from the vacuum, by sweeping a driving frequency through the vacuum-to-bound-state resonance. Anharmonicity suppresses unwanted multi-particle creation.

## The trick
Apply a time-dependent source $J_1(t,x) = f(t)h(x)$ where:

$$f(t) = g\cos(\omega_0 t + Bt^2/T), \quad -T/2 \leq t \leq T/2$$

This is a chirped pulse whose instantaneous frequency sweeps linearly from $\omega_0 - B/2$ to $\omega_0 + B/2$ over time $T$, crossing the vacuum-to-bound-state resonance at $\omega_0 = m - B_{\text{bind}}$ (where $B_{\text{bind}}$ is the binding energy).

In the rotating frame, this reduces to a two-level adiabatic crossing problem. If the sweep is slow enough, the system evolves from vacuum to the single-particle bound state with high fidelity.

**Why multi-particle creation is suppressed:** The $\lambda\phi^4$ interaction makes the energy spectrum anharmonic — the two-particle bound state energy is $2\omega - \delta$ (shifted by $O(\lambda)$ from $2\omega$). Choosing the bandwidth $B = O(\lambda)$ ensures the two-particle transition lies outside the frequency band where the source has support:

$$\tilde{F}(\omega) \approx g\sqrt{T/B} \quad \text{for } |\omega - \omega_0| < B/2, \quad \tilde{F}(\omega) = O(g/B) \quad \text{otherwise}$$

The spatial profile $h(x)$ is chosen to match the bound-state wavefunction $\psi_0(x)$, making the matrix element for exciting unbound continuum states vanish.

**Conditions for success (error $\varepsilon$):**
- Adiabaticity: $B^2/(T\Omega^3) = O(\varepsilon)$
- Rotating-wave approximation: $\Omega/\omega_0 = O(\varepsilon)$
- Multi-particle suppression: $g/B = O(\varepsilon)$ and $\lambda(g\sqrt{T/B})^3 = O(\varepsilon)$
- Satisfied by: $g \sim \varepsilon^5$, $\lambda \sim B \sim \varepsilon^4$, $T \sim 1/\varepsilon^8$

## When to reach for it
- Preparing specific particle-number states from vacuum in interacting QFTs
- Any BQP-hardness construction that needs to start from vacuum rather than a pre-prepared initial state
- Precision spectroscopy or state transfer problems where anharmonicity can be exploited to suppress leakage

## Complexity
Time $O(1/\varepsilon^8)$ per qubit (parallelisable). For $n$ qubits with $\varepsilon = 1/n$: total time $O(n^8)$.

## Caveat
Requires perturbative control over the anharmonic spectrum — specifically, the $O(\lambda)$ shift of multi-particle energies must be accurately known to set $B$ correctly. The technique is non-perturbative in the sense that it resums an infinite class of Feynman diagrams (the Rabi oscillations), but the *error analysis* relies on perturbation theory, which is assumed but not rigorously proven in the presence of time-dependent sources.

## Related notes
- [[BQP-Completeness of Scattering in Scalar Quantum Field Theory (Jordan-Krovi-Lee-Preskill 2018) — Paper Notes]]
- [[Qubit Encoding in Double-Well Potentials]]
- [[Adiabatic Wavepacket Preparation with Phase Cancellation]]
- [[Adiabatic State Preparation for Chemistry]]
