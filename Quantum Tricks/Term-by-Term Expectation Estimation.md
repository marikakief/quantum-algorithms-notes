# Term-by-Term Expectation Estimation

> **Source:** Knill, Ortiz & Somma, PRA 75, 012328 (2007)
> **Tags:** #trick #measurement #expectation-value #NISQ

## What it does

Estimates $\langle\psi|H|\psi\rangle$ for $H = \sum_j h_j H_j$ by measuring each term independently and adding classically. No long coherent evolution needed — each measurement is a separate short experiment.

## The trick

1. Decompose $H = \sum_{j=1}^M h_j H_j$ where each $H_j$ is efficiently measurable (e.g., a tensor product of Pauli operators)
2. For each $j$: prepare $|\psi\rangle$, measure $H_j$, repeat to estimate $\langle H_j \rangle$ to precision $\delta_j$
3. Compute $\langle H \rangle = \sum_j h_j \langle H_j \rangle$

Total cost: $O(\|h\|_1^2 / p^2)$ repetitions for precision $p$ in $\langle H \rangle$, where $\|h\|_1 = \sum_j |h_j|$.

## When to reach for it

- Near-term / NISQ algorithms where coherence time is limited (e.g., [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|VQE]])
- Any setting where you need $\langle H \rangle$ but can't afford the long coherent evolution of [[Gapped Phase Estimation|phase estimation]]
- Benchmarking quantum devices: prepare a state, measure energy

## Complexity

$O(M \cdot \|h\|_{\max}^2 / p^2)$ total measurements. Each measurement requires only preparing $|\psi\rangle$ once and measuring in a Pauli basis — $O(1)$ circuit depth beyond state preparation.

## Caveat

The $1/p^2$ scaling is the standard sampling bottleneck. For chemistry precision ($p \sim 10^{-3}$ Ha) with $M \sim N^4$ terms, the total measurement count can be enormous. [[Gapped Phase Estimation|Phase estimation]] and [[QSVT Meta-Template|QSVT]] approaches achieve $\log(1/p)$ precision scaling but require long coherent circuits.

## Related notes

- [[Quantum Simulations of Classical Annealing Processes (Knill-Ortiz-Somma 2007) — Paper Notes]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]
- [[Gapped Phase Estimation]]
