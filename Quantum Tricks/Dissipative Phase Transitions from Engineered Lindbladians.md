# Dissipative Phase Transitions from Engineered Lindbladians

> **Source:** Verstraete, Wolf, Cirac, arXiv:0803.1447
> **Tags:** #trick #dissipation #phase-transitions #Lindbladian #topological-order

## What it does
Shows that quantum phase transitions can be driven entirely by dissipation — no Hamiltonian required. By tuning parameters of a Lindbladian, the steady state can undergo abrupt changes in its physical properties (order parameter, topological order, correlation structure).

## The trick
The [[Local Projection Channel for Frustration-Free Ground States|dissipative state engineering]] framework prepares ground states of frustration-free Hamiltonians as steady states. Frustration-free Hamiltonians can exhibit quantum phase transitions (established by Wolf-Ortiz-Verstraete-Cirac 2006 in 1D and Verstraete-Wolf-Perez-Garcia-Cirac 2006 in 2D). Therefore:

1. Parameterise a family of frustration-free Hamiltonians $H(\alpha)$ that undergoes a quantum phase transition at some $\alpha = \alpha_c$
2. Construct the corresponding family of cp-maps $T_\alpha$ or Lindbladians $\mathcal{L}_\alpha$
3. As $\alpha$ varies through $\alpha_c$, the steady state of $\mathcal{L}_\alpha$ changes abruptly

This is conceptually different from the standard condensed-matter setting where phase transitions occur in Hamiltonian ground states. Here, the phase transition is a property of the *dissipative dynamics* — the attractor of the open system evolution changes character.

## When to reach for it
- Studying open quantum systems where the steady state (not the ground state of a Hamiltonian) exhibits phase transitions
- Designing experiments to observe quantum phase transitions in engineered dissipative systems (optical lattices, trapped ions)
- Theoretical: the classification of phases of matter may need to be extended to include dissipative phases

## Complexity
Preparing the steady state near the phase transition costs $O(N \log N)$ for commuting cases. Detecting the transition requires measuring an order parameter and varying $\alpha$.

## Caveat
The framework is limited to frustration-free Hamiltonians, which constrains the types of phase transitions that can be realised. Also, the convergence time may diverge near the phase transition (the dissipative gap closes), analogous to critical slowing down in classical phase transitions. The paper doesn't analyse the critical behaviour of the gap.

## Related notes
- [[Quantum Computation and Quantum-State Engineering Driven by Dissipation (Verstraete-Wolf-Cirac 2009) — Paper Notes]]
- [[Local Projection Channel for Frustration-Free Ground States]]
- [[Fault-Tolerant Quantum Computation by Anyons (Kitaev 2003) — Paper Notes]]
- [[Projected Entangled Pairs as Variational Ansatz]]
