
> **Source:** Earl T. Campbell, *A random compiler for fast Hamiltonian simulation*, arXiv:1811.08017, Phys. Rev. Lett. **123**, 070503 (2019)  
> **Links:** [arXiv](https://arxiv.org/abs/1811.08017) · [PRL](https://doi.org/10.1103/PhysRevLett.123.070503)  
> **Tags:** #trick #qdrift #randomization #hamiltonian-simulation

## Idea

For $H = \sum_j h_j H_j$ with $\|H_j\| = 1$, set
$$
p_j = \frac{|h_j|}{\lambda}, \qquad \lambda = \sum_j |h_j|, \qquad \tau = \frac{t\lambda}{N}.
$$
Repeat $N$ times: sample $j \sim p_j$, apply $e^{-i\tau\,\mathrm{sgn}(h_j) H_j}$.

The expected channel over $N$ steps approximates $e^{-iHt}$ with diamond-norm error $O((\lambda t)^2/N)$, so diamond-norm error $\epsilon$ requires $N = O((\lambda t)^2/\epsilon)$.

Cost is independent of $L$ and of the largest individual coefficient $\Lambda$ — only $\lambda = \sum_j |h_j|$ matters.

## Why this is useful

Heavy terms get sampled often; negligible terms are automatically down-weighted. For dense Pauli decompositions in quantum chemistry, $\lambda$ can be much smaller than $\Lambda L$, giving a genuine circuit-depth saving over naïve [[Order-Condition Cancellation in Product Formulas|Trotter]].

## When not to use it

- Deterministic ordering exploits locality or commutator structure (use [[Trotter Commutator-Scaling Bound|Trotter with commutator bounds]] instead).
- $\lambda \approx \Lambda L$ (no advantage over Trotter).
- High precision regime ($\epsilon \ll 1$) — the $1/\epsilon$ rather than $1/\epsilon^{1/p}$ scaling loses to higher-order methods.
- You need a deterministic circuit for verification purposes.

## Comparison

```
Deterministic Trotter:
  e^{-ih_1 H_1 δt} e^{-ih_2 H_2 δt} ⋯ e^{-ih_L H_L δt}   [all L terms, fixed order]

[[Term-Weighted Random Hamiltonian Sampling (qDRIFT)|qDRIFT]]:
  pick j ~ p_j, apply e^{-iτ sgn(h_j) H_j}                 [one term, random, repeat N times]
```

## Related notes

- [[qDRIFT Randomized Hamiltonian Simulation (Campbell 2018) — Paper Notes]]
- [[Fixed-Angle Randomized Product Formula]]
- [[Diamond-Norm Channel Averaging for Random Compilers]]
- [[Error-Budget Allocation Across Controlled-Time Schedules]]
