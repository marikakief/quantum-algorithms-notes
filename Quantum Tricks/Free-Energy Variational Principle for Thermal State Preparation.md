# Free-Energy Variational Principle for Thermal State Preparation

> **Source:** Chowdhury, Low, Wiebe, arXiv:2002.00055
> **Tags:** #trick #gibbs-state #variational #free-energy

## What it does
Turns Gibbs-state preparation into a variational optimisation problem by minimising free energy instead of energy.

## The trick
Use

$$
F_\beta(\rho) = \operatorname{Tr}(H\rho) - \beta^{-1}S(\rho)
$$

as the objective. Its unique minimiser is the Gibbs state $\rho_\beta \propto e^{-\beta H}$. This converts thermal-state preparation into an optimisation problem over an ansatz family of mixed states or purifications.

## When to reach for it
When you want a variational route to finite-temperature states on hardware that cannot support long coherent fault-tolerant routines.

## Complexity
Cost is dominated by estimating both the energy and entropy terms across many optimisation rounds.

## Caveat
The principle is exact, but the algorithm is only as good as the ansatz and the entropy estimator.

## Related notes
- [[A Variational Quantum Algorithm for Preparing Quantum Gibbs States (Chowdhury-Low-Wiebe 2020) — Paper Notes]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]