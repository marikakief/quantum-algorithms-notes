# Golden-Thompson Bound for Quantum Objective Functions

> **Source:** Mária Kieferová and Nathan Wiebe, arXiv:1612.05204; also Amin et al., arXiv:1601.02036  
> **Tags:** #trick #quantum-machine-learning #Boltzmann-machine #matrix-inequality #optimisation

## What it does

Replaces an intractable quantum log-likelihood objective with a computable lower bound via the Golden-Thompson inequality, enabling gradient-based training when the Hamiltonian doesn't commute with the POVM elements.

## The trick

The Golden-Thompson inequality states: for Hermitian matrices $A, B$,

$$\text{Tr}[e^{A+B}] \geq \text{Tr}[e^A e^B]$$

with equality iff $[A, B] = 0$.

For quantum Boltzmann machine training with POVM elements $\Lambda_v$, the log-likelihood involves $\log \text{Tr}[\Lambda_v e^{-H}]$, which requires computing $\text{Tr}[\Lambda_v e^{-H}]$ — intractable when $[\Lambda_v, H] \neq 0$ because Duhamel's formula gives a time-ordered integral.

Apply Golden-Thompson with $A = -H + \log \Lambda_v$:

$$\text{Tr}[\Lambda_v e^{-H}] = \text{Tr}[e^{-H + \log \Lambda_v}] \geq \text{Tr}[e^{-H_v}]$$

where $H_v = H - \log \Lambda_v$. This gives a lower bound on the log-likelihood that *is* tractable: its gradient is a difference of thermal expectations in $e^{-H_v}/Z_v$ and $e^{-H}/Z$, both of which can be estimated by Gibbs sampling.

The bound is tight when $[\Lambda_v, H] = 0$ — i.e., when the measurement basis commutes with the Hamiltonian, recovering the classical Boltzmann machine gradient exactly.

## When to reach for it

- Training quantum Boltzmann machines when the objective function involves $\text{Tr}[e^{A+B}]$ for non-commuting $A, B$
- Any quantum optimisation problem where you need a tractable lower bound on a matrix exponential trace
- When the target and model nearly commute, the bound is tight and the gradients are nearly exact

## Complexity

Same as the exact training: $O(M^2/\varepsilon^2)$ Gibbs samples per gradient step. The cost is in computing expectations in two thermal states ($e^{-H_v}/Z_v$ and $e^{-H}/Z$) per POVM element.

## Caveat

- The gap between the bound and the true objective can be large when $\|[H, \log \Lambda_v]\|$ is large. In this regime, gradients are biased and convergence can stall.
- For computational basis measurements ($\Lambda_v = |y_v\rangle\langle y_v|$), the gradient of the lower bound has $\text{Tr}[e^{-H_v}\partial_\theta H] = 0$ for off-diagonal quantum terms. This means Golden-Thompson with classical data *cannot learn quantum terms* — you need non-diagonal POVMs or relative entropy training.
- Commutator training (truncated Hadamard expansion) can be more accurate but less numerically stable.

## Related notes

- [[Tomography and Generative Training with Quantum Boltzmann Machines (Kieferová-Wiebe 2017) — Paper Notes]]
- [[Relative Entropy Gradient for Quantum Model Training]]
- [[Quantum Metropolis Sampling (Temme-Osborne-Vollbrecht-Poulin-Verstraete 2011) — Paper Notes]]
