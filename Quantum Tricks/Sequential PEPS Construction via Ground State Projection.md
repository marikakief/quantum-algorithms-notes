# Sequential PEPS Construction via Ground State Projection

> **Source:** Schwarz, Cubitt, Temme, Verstraete, Perez-Garcia, arXiv:1211.4050; Schwarz, Temme, Verstraete, PRL 101:110502 (2012)
> **Tags:** #trick #PEPS #state-preparation #tensor-networks #quantum-circuits

## What it does
Constructs a many-body quantum state (a PEPS) on a quantum computer by starting from an easily preparable initial state and sequentially projecting onto ground states of partially constructed parent Hamiltonians.

## The trick
A PEPS on $N$ sites has parent Hamiltonians $H_t$ ($t = 0, 1, \ldots, N$) where $H_t$ is the parent Hamiltonian for the PEPS with the first $t$ sites "filled in" (projectors applied) and the remaining sites still in the G-isometric initial state.

The algorithm:
1. **Prepare the initial state** — for injective PEPS this is just maximally entangled pairs; for G-injective PEPS, prepare the G-isometric PEPS using the Aguado-Vidal quantum double circuit
2. **For $t = 1$ to $N$:** project from the ground space of $H_t$ to the ground space of $H_{t+1}$ using:
   - Quantum phase estimation on $e^{-iH_{t+1}\tau}$ to implement the measurement $\{P_{t+1}, P_{t+1}^\perp\}$
   - [[Marriott-Watrous Rewinding for Degenerate Ground Spaces|Marriott-Watrous rewinding]] to boost success probability

The PEPS structure guarantees that the overlap between successive ground spaces is bounded away from zero (by the condition number of the PEPS tensors), so each step succeeds in polynomial time.

This is analogous to adiabatic state preparation but uses discrete projective measurements instead of continuous Hamiltonian interpolation. The "jagged adiabatic lemma" of [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes|Aharonov-Ta-Shma]] shows that a continuous path between the Hamiltonians exists, so adiabatic preparation is an alternative.

## When to reach for it
- Preparing tensor network states (MPS, PEPS) on a quantum computer when direct circuit construction isn't available
- Situations where the target state has a frustration-free parent Hamiltonian with a known gap
- Constructing topological states (toric code, RVB, quantum doubles) that don't have simple product-state descriptions

## Complexity
Total runtime: $O(N^4 \kappa_G^2 \Delta^{-1} \varepsilon^{-1})$ where $N$ is the number of sites, $\kappa_G$ the condition number on the symmetric subspace, $\Delta$ the minimum spectral gap, and $\varepsilon$ the target error. The $N^4$ comes from: $N$ sites $\times$ $O(N\kappa_G^2/\varepsilon)$ measurements per site $\times$ $\tilde{O}(N^2/\Delta)$ per measurement (phase estimation).

## Caveat
- Requires polynomial spectral gap throughout the sequence — this is an assumption, not proven in general
- Each QPE measurement is an expensive subroutine (involves [[Hamiltonian simulation]])
- For MPS (1D), direct sequential circuit construction via Verstraete-Cirac-Latorre is simpler and avoids the gap assumption; this technique is mainly needed for 2D PEPS where direct circuits aren't known
- Doesn't give control over which topological sector the final state lands in

## Related notes
- [[Preparing Topological PEPS on a Quantum Computer (Schwarz-Cubitt-Verstraete 2012) — Paper Notes]]
- [[Marriott-Watrous Rewinding for Degenerate Ground Spaces]]
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]]
- [[Efficient Classical Simulation of Slightly Entangled Quantum Computations (Vidal 2003) — Paper Notes]]
- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]]
