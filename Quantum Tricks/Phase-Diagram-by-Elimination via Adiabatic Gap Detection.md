# Phase-Diagram-by-Elimination via Adiabatic Gap Detection

> **Source:** Wecker, Hastings, Wiebe, Clark, Nayak, Troyer, arXiv:1506.05135
> **Tags:** #trick #adiabatic #phase-diagram #Hubbard #state-preparation #gap-detection

## What it does
Determines the ground-state phase of a quantum many-body system by testing candidate phases via adiabatic evolution, using gap closure as the diagnostic for incorrect guesses.

## The trick
For each candidate phase (e.g., superconductor, antiferromagnet, stripe order):

1. Construct a simple mean-field Hamiltonian $H_{\text{MF}}$ whose ground state is a representative of that phase (a Slater determinant or BCS state)
2. Prepare the mean-field ground state on the quantum computer (via [[Givens Rotation Slater Determinant Preparation|Givens rotations]])
3. Adiabatically interpolate: $H(s) = (1-s) H_{\text{MF}} + s H_{\text{target}} + \text{small symmetry-breaking fields}$
4. Use [[Iterative Phase Estimation (Kitaev)|phase estimation]] to check if the gap closes during the evolution

If the gap stays open, the candidate phase is adiabatically connected to the target — you guessed right. If it closes (detected as failed QPE projection), you guessed wrong. Enumerate candidates until one succeeds.

The symmetry-breaking fields serve two purposes: (a) gap out Goldstone modes in ordered phases, (b) gap out nodal fermions in $d$-wave superconductors. They must be small enough not to bias the physics.

## When to reach for it
- Mapping phase diagrams of strongly correlated lattice models (Hubbard, $t$-$J$, frustrated magnets)
- When candidate phases are known from mean-field theory or experiment but the correct ground state is uncertain
- When the system is expected to have a conventional ordered phase describable by mean-field theory

## Complexity
- State preparation: $O(N_e N)$ for [[Givens Rotation Slater Determinant Preparation|Givens rotations]]
- Adiabatic evolution: $O(1/\Delta^2)$ for gap $\Delta$ (improved to subpolynomial with [[Boundary Cancellation Schedule for Adiabatic State Preparation|boundary cancellation]])
- QPE verification: $O(1/\Delta)$ per check
- Number of candidates: depends on physics — typically $O(1)$ for standard condensed matter systems

## Caveat
Fails for exotic phases that don't have simple mean-field descriptions (topological order, spin liquids without local order parameters). Also fails near first-order transitions where the gap closes abruptly. The approach assumes you can enumerate the relevant candidate phases — if the actual ground state is something you didn't think of, you won't find it this way.

## Related notes
- [[Solving Strongly Correlated Electron Models on a Quantum Computer (Wecker, Hastings, Wiebe, Clark, Nayak, Troyer 2015) — Paper Notes]]
- [[Adiabatic State Preparation for Chemistry]]
- [[Boundary Cancellation Schedule for Adiabatic State Preparation]]
- [[Givens Rotation Slater Determinant Preparation]]
