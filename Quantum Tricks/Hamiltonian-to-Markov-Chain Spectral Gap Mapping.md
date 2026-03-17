# Hamiltonian-to-Markov-Chain Spectral Gap Mapping

> **Source:** Aharonov, van Dam, Kempe, Landau, Lloyd, Regev, arXiv:quant-ph/0405098, Section 2.3
> **Tags:** #trick #spectral-gap #markov-chains #conductance

## What it does

Converts the problem of lower-bounding a Hamiltonian's spectral gap into a classical Markov chain mixing problem, where conductance bounds (Cheeger inequality) apply.

## The mapping

Given a Hamiltonian $H$ on an $(L+1)$-dimensional space, define $G = I - H$. Suppose $G$ has non-negative entries and its largest eigenvalue $\mu > 0$ has a unique eigenvector $(\alpha_0, \ldots, \alpha_L)$ with all $\alpha_i > 0$ (guaranteed by Perron's theorem when some power of $G$ has all positive entries).

Construct the stochastic matrix:

$$
P_{ij} = \frac{\alpha_j}{\mu \alpha_i} G_{ij}
$$

Then:
- $P$ is a valid stochastic matrix (rows sum to 1, entries non-negative)
- Stationary distribution: $\pi_i = \alpha_i^2 / Z$ where $Z = \sum_i \alpha_i^2$
- **Eigenvalue gap of $P$** $=$ **$\Delta(H) / (1 - \lambda)$** where $\lambda$ is the ground energy of $H$

So bounding the spectral gap of $H$ reduces to bounding the spectral gap of $P$.

```
  Hamiltonian H                    Stochastic matrix P
  ┌──────────────┐                 ┌──────────────┐
  │ G = I - H    │  ──Eq.(3)──▶   │ P_{ij} =     │
  │ non-negative │                 │ (αⱼ/μαᵢ)Gᵢⱼ │
  │ entries      │                 │              │
  └──────────────┘                 └──────────────┘
        │                                │
   Δ(H) = ?                     gap(P) ≥ ½ϕ(P)²
        │                                │
        └──── gap(P)·(1-λ) = Δ(H) ──────┘
```

## Why this helps

Classical Markov chain theory offers tools that don't have direct quantum analogues:

- **Conductance bound (Cheeger):** $\text{gap}(P) \geq \frac{1}{2}\phi(P)^2$ where $\phi(P) = \min_B F(B)/\pi(B)$ is the conductance
- Conductance arguments often need only rough knowledge of $\pi$ (e.g., [[Monotonicity Bootstrap for Conductance Bounds|monotonicity]]), not exact values

## When to use it

- Hamiltonian has a natural random-walk interpretation (tridiagonal / path-like structure)
- The restricted Hamiltonian $H_{S_0}$ is a graph Laplacian or close to one (as in the [[History-State Encoding with Unary Clock|history-state construction]])
- You need gap bounds but can't diagonalize exactly

## Caveat

Only works when $G = I - H$ has non-negative entries. This is not always the case — it works for the path-Laplacian structure that arises in [[History-State Encoding with Unary Clock|history-state constructions]], but not for general Hamiltonians.

## Related notes

- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Monotonicity Bootstrap for Conductance Bounds]]
- [[History-State Encoding with Unary Clock]]
- [[Perturbation Lemma for Locality Reduction]]
