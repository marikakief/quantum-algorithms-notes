# Interaction Picture LCHS for Fast-Forwardable Dissipation

> **Source:** Dong An, Jin-Peng Liu, and Lin Lin, arXiv:2303.01029
> **Tags:** #trick #LCHS #interaction-picture #fast-forwarding #open-quantum-systems

## What it does
Eliminates the $K = O(1/\varepsilon)$ frequency-cutoff overhead in [[LCHS Kernel for Non-Unitary Dynamics|LCHS]] when the dissipative (Hermitian) part $L$ of the generator is fast-forwardable.

## The trick
In the standard LCHS, each unitary $U_j(T) = \mathcal{T}e^{-i\int_0^T (H(s) + k_jL(s))ds}$ has norm scaling with $k_j \|L\|$, requiring a Trotter step count that grows with $K$. But if $e^{-ikLt}$ can be implemented at cost independent of $k$ and $t$ (e.g., $L$ is diagonal), switch to the interaction picture:

$$\mathcal{T}e^{-\int_0^t A(s)ds} \approx \sum_j c_j e^{-ik_jLt} \left(\mathcal{T}e^{-i\int_0^t H_I(s;k_j)ds}\right) e^{ik_jLt},$$

where $H_I(s;k) = e^{ikLs}H(s)e^{-ikLs}$.

Now the expensive part is simulating $H_I$ via the truncated Dyson series, whose cost depends on $\|H_I\| = \|H\|$ (independent of $k$!) and only *logarithmically* on $\|H_I'\|$ (which scales with $K$). The $K$-dependence drops from polynomial to polylogarithmic.

## When to reach for it
- Open quantum systems with complex absorbing potentials ($L = V_I$ is diagonal and non-negative).
- Any non-unitary dynamics where the dissipative part has a known, cheaply-implementable spectral decomposition.
- Schrödinger equations with imaginary potentials used for boundary treatment.

## Complexity
Matrix queries: $\tilde{O}(qT\max_t\|H(t)\| \cdot \mathrm{polylog}(\|V_I\|/\varepsilon))$, eliminating the $1/\varepsilon$ scaling that plagues standard LCHS.

## Caveat
Requires $L$ to be fast-forwardable — $e^{-ikLs}$ must be implementable in $O(1)$ cost for all $k, s$. This holds for diagonal $L$ but not in general. When $L$ is dense and non-commuting with $H$, the interaction picture doesn't help and you're stuck with the Cauchy kernel's $O(1/\varepsilon)$ cutoff (until you switch to a [[Near-Optimal Hardy-Space Kernel for LCHS|better kernel]]).

## Related notes
- [[Linear Combination of Hamiltonian Simulation for Non-Unitary Dynamics (An-Liu-Lin 2023) — Paper Notes]]
- [[LCHS Kernel for Non-Unitary Dynamics]]
- [[Dyson Series Simulation in the Interaction Picture (Low-Wiebe 2018) — Paper Notes]] — the interaction picture simulation technique used here
- [[Near-Optimal Hardy-Space Kernel for LCHS]] — the alternative fix that replaces the kernel rather than the simulation picture
