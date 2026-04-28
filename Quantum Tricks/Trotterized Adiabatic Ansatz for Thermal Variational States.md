# Trotterized Adiabatic Ansatz for Thermal Variational States

> **Source:** Chowdhury, Low, Wiebe, arXiv:2002.00055
> **Tags:** #trick #ansatz #adiabatic #trotter #gibbs-state #variational

## What it does
Uses an adiabatic-style circuit, discretised into Trotter steps, as a variational family for approximate thermal states.

## The trick
Choose a Hamiltonian path from an easy reference problem to the target and parameterise a circuit by Trotterized segments along that path. Purify the state if needed, then optimise the segment parameters against a free-energy objective.

## When to reach for it
When adiabatic structure captures the physics you want, but you need a shallow tunable circuit rather than a fixed long adiabatic schedule.

## Complexity
Circuit depth scales with the number of Trotter segments and the cost of each segment; classical optimisation adds another outer loop.

## Caveat
If the adiabatic path is poor or the Trotterization is too coarse, the ansatz may miss the relevant thermal manifold.

## Related notes
- [[A Variational Quantum Algorithm for Preparing Quantum Gibbs States (Chowdhury-Low-Wiebe 2020) — Paper Notes]]
- [[Adiabatic State Preparation for Chemistry]]