# Importance Sampling over Hamiltonian Terms

> **Source:** Knill, Ortiz & Somma, PRA 75, 012328 (2007); also implicit in Campbell (2019) qDRIFT
> **Tags:** #trick #measurement #sampling #variance-reduction

## What it does

Reduces the variance in [[Term-by-Term Expectation Estimation|term-by-term expectation estimation]] by sampling Hamiltonian terms with probability proportional to their coefficient magnitude $|h_j|$, rather than measuring all terms uniformly.

## The trick

For $H = \sum_{j=1}^M h_j H_j$ with $\|H_j\| \leq 1$:

Instead of measuring all $M$ terms with equal effort, sample term $j$ with probability $p_j = |h_j| / \|h\|_1$ and estimate:

$$
\langle H \rangle = \|h\|_1 \cdot \mathbb{E}_{j \sim p}\left[\mathrm{sgn}(h_j) \langle H_j \rangle\right]
$$

The variance of this estimator is bounded by $\|h\|_1^2$, independent of $M$. This means the measurement cost depends on the $\ell_1$ norm $\|h\|_1 = \sum_j |h_j|$ rather than the number of terms $M$.

## Connection to qDRIFT

The same importance sampling distribution $p_j = |h_j|/\|h\|_1$ appears in [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]], where it determines which term to apply at each random simulation step. The measurement and simulation uses are dual: one samples to estimate $\langle H \rangle$, the other samples to simulate $e^{-iHt}$.

## When to reach for it

- Estimating $\langle H \rangle$ when $M$ is large but many $|h_j|$ are small
- Chemistry Hamiltonians where the $\ell_1$ norm $\|h\|_1 \ll M \cdot \max_j |h_j|$
- Any [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes|VQE]]-type algorithm where measurement budget is the bottleneck

## Complexity

$O(\|h\|_1^2 / p^2)$ total samples for precision $p$. Compare with naive uniform measurement: $O(M \cdot \max_j |h_j|^2 / p^2)$. When $\|h\|_1 \ll M \cdot \max_j |h_j|$, importance sampling wins.

## Caveat

Still has the $1/p^2$ sampling bottleneck. For high precision, [[Gapped Phase Estimation|phase estimation]] or [[QSVT Meta-Template|QSVT]] approaches with $\log(1/p)$ scaling are preferable — they just need longer circuits.

## Related notes

- [[Quantum Simulations of Classical Annealing Processes (Knill-Ortiz-Somma 2007) — Paper Notes]]
- [[Term-by-Term Expectation Estimation]]
- [[Term-Weighted Random Hamiltonian Sampling (qDRIFT)]]
- [[A Variational Eigenvalue Solver on a Quantum Processor (Peruzzo-McClean et al. 2014) — Paper Notes]]
