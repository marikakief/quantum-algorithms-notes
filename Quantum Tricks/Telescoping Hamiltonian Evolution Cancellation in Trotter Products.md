# Telescoping Hamiltonian Evolution Cancellation in Trotter Products

> **Source:** Ding, Chen & Lin, arXiv:2308.15676 (Proposition 5)
> **Tags:** #trick #Trotter #Hamiltonian-simulation #circuit-optimisation

## What it does
Reduces the total Hamiltonian simulation cost in a Trotter product where each factor involves conjugation by $e^{\pm iH s_l}$ at different times $s_l$, by cancelling consecutive forward-backward evolutions.

## The trick

Consider a time-ordered product of operators at discretised times $s_l = l\tau_s$:

$$\overrightarrow{\prod_l} A_l(s_l) = \overrightarrow{\prod_l}\, e^{iHs_l} A_l\, e^{-iHs_l}$$

Naively, each factor requires simulating $e^{iHs_l}$ and $e^{-iHs_l}$, for a total cost scaling as $O(M \cdot S)$ where $M$ is the number of terms and $S = M\tau_s$ is the total time range.

But consecutive factors telescope:

$$e^{-iHs_l} \cdot e^{iHs_{l+1}} = e^{iH\tau_s}$$

So the product rewrites as:

$$\overrightarrow{\prod_l} A_l(s_l) = e^{-iHS_s}\left(\overrightarrow{\prod_l} A_l\, e^{iH\tau_s}\right) e^{iH(S_s + \tau_s)}$$

Each interior step costs only $O(\tau_s)$ Hamiltonian simulation instead of $O(s_l)$. Total cost: $O(M \cdot \tau_s) = O(S_s)$ — **independent of the ordering of the $s_l$ values**.

For second-order Trotter (forward then backward products), the outer $e^{\pm iHS_s}$ factors from consecutive time steps also cancel, leaving only short $e^{\pm iH\tau_s}$ evolutions throughout.

## When to reach for it
- Any Trotter splitting where factors involve Heisenberg-picture operators $A(s) = e^{iHs}Ae^{-iHs}$ at a sequence of times
- Lindbladian simulation via quadrature (the motivating application)
- Interaction-picture simulation methods
- Dyson series implementations with multiple time integrals
- Any circuit with alternating $e^{iHt}$ and $e^{-iHt}$ that can be reordered

## Complexity
- **Without cancellation:** $O(M \cdot S)$ total Hamiltonian evolution time
- **With cancellation:** $O(S)$ total — a factor of $M$ improvement
- No additional ancilla or gate overhead; purely a circuit rearrangement

## Caveat
Requires the times $s_l$ to be evenly spaced (or at least ordered) so that consecutive cancellations are possible. If the time points are irregular, partial cancellation may still help but the full telescoping doesn't apply.

## Related notes
- [[Single-Ancilla Ground State Preparation via Lindbladians (Ding-Chen-Lin 2023) — Paper Notes]]
- [[Coherent Time-Ordering via Ancilla Clocks]]
- [[Collision Budgeting in Discretized Ordered Integrals]]
