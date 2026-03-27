
> **Source:** Lee, Babbush, Chan et al., arXiv:2208.02199 (2022)
> **Tags:** #trick #state-preparation #adiabatic #phase-estimation #quantum-chemistry #orthogonality-catastrophe

## What it does

Establishes an empirical relationship between the adiabatic state preparation (ASP) time and the overlap of the initial state with the target ground state: $T_{\mathrm{ASP}} \sim \mathrm{poly}(1/|\langle \Upsilon_0 | \Psi_0 \rangle|)$, mediated by the minimum spectral gap along the adiabatic path.

## The trick

For an adiabatic path $H(s) = (1-s)H_{\mathrm{init}} + s H_{\mathrm{final}}$, the estimated ASP time is $T_{\mathrm{ASP}}^{\mathrm{est}} \sim \max_s \tau(s)$ where:

$$\tau(s) = \frac{|\langle \Upsilon_0(s) | dH(s)/ds | \Upsilon_1(s) \rangle|}{\Delta(s)^2}$$

with $\Delta(s)$ the ground-state gap and $\Upsilon_0(s), \Upsilon_1(s)$ the ground and first excited states of $H(s)$.

The empirical finding on [2Fe-2S] clusters (24 qubits, over 100 initial Hamiltonians): the minimum gap along the path scales as $1/\min_s \Delta(s) \sim \mathrm{poly}(1/|\langle \Upsilon_0 | \Psi_0 \rangle|^2)$, and consequently $T_{\mathrm{ASP}} \sim \mathrm{poly}(1/|\langle \Upsilon_0 | \Psi_0 \rangle|)$.

This held for two qualitatively different families of initial Hamiltonians (mean-field Slater determinant ground states and interacting Dyall Hamiltonians with varying active space sizes), and the data from both fell on the same curve. The behaviour is reminiscent of Grover-like unstructured search, where the adiabatic gap scales as $\Delta \sim |\langle \Upsilon_0 | \Psi_0 \rangle|$.

The practical consequence: ASP doesn't give you a free lunch. Getting a good initial state is the hard part, and the ASP time reflects exactly how hard that is.

## When to reach for it

- Evaluating whether [[Stroboscopic Adiabatic Walk|adiabatic state preparation]] is practical for a specific chemical system
- Resource estimation for fault-tolerant quantum chemistry: the state preparation overhead may dominate the QPE circuit cost
- Comparing ASP cost against classical heuristic cost for the same problem — if classical heuristics can find a good starting point cheaply, ASP's polynomial overhead doesn't provide an exponential advantage
- Any quantum algorithm that assumes "a good initial state can be prepared efficiently" — this gives concrete data on when that assumption holds and when it breaks

## Complexity

The ASP time itself is $T_{\mathrm{ASP}} \sim \mathrm{poly}(1/|\langle \Upsilon_0 | \Psi_0 \rangle|) / \Delta_{\min}^2$. For linear interpolation paths on chemical Hamiltonians, the inverse gap dominates and is correlated with the initial overlap. Total QPE cost including ASP: $\mathrm{poly}(1/S) \cdot [T_{\mathrm{QPE}} + T_{\mathrm{ASP}}]$, where the $\mathrm{poly}(1/S)$ repetition cost compounds the ASP overhead.

## Caveat

- This is empirical, demonstrated on a single (small) chemical system. The scaling might differ for other chemical families, though the theoretical connection to unstructured search is suggestive.
- The linear interpolation path is a naive choice. Optimized paths (e.g., geodesic, locally adiabatic schedules) could reduce $T_{\mathrm{ASP}}$, but would need $T_{\mathrm{ASP}} = o(\mathrm{poly}(1/S))$ to change the qualitative conclusion.
- Getting $T_{\mathrm{ASP}} < T_{\mathrm{QPE}}$ required starting from an interacting Hamiltonian with 20 of 24 active qubits — at which point finding the initial ground state is almost as hard as the original problem.

## Related notes
- [[Is There Evidence for Exponential Quantum Advantage in Quantum Chemistry (Lee, Babbush, Chan et al 2022) — Paper Notes]] — source paper
- [[Postponing the Orthogonality Catastrophe (Tubman, Mejuto-Zaera, Babbush et al 2018) — Paper Notes]] — complementary data on ground-state overlaps
- [[Stroboscopic Adiabatic Walk]] — adiabatic walk construction for qubitized quantum simulation
- [[ASCI Overlap Estimation for QPE Initial State]] — classical overlap certification
- [[EQA Assessment Framework (State Preparation vs Classical Heuristic)]] — the broader framework this observation feeds into
