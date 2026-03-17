# Monotonicity Bootstrap for Conductance Bounds

> **Source:** Aharonov, van Dam, Kempe, Landau, Lloyd, Regev, arXiv:quant-ph/0405098, Claim 3.7
> **Tags:** #trick #spectral-gap #markov-chains #monotonicity

## What it does

Shows that the ground state of a tridiagonal Hamiltonian has monotonically decreasing components, which is exactly the structural property needed to make conductance bounds work — without knowing the ground state explicitly.

## The trick

Let $G(s) = I - H_{S_0}(s)$ be the non-negative matrix from the [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping|Hamiltonian-to-Markov-chain mapping]], with largest eigenvalue $\mu$ and ground state $(\alpha_0, \ldots, \alpha_L)$.

**Claim:** $\alpha_0 \geq \alpha_1 \geq \cdots \geq \alpha_L \geq 0$.

**Proof:**
1. The ground state is the limit: $(\alpha_0, \ldots, \alpha_L) \propto \lim_{k \to \infty} (G/\mu)^k (1, \ldots, 1)^\top$
2. $G$ preserves monotonicity: if $v$ is monotone (non-increasing), then $Gv$ is monotone (verify directly from the tridiagonal structure)
3. $(1, \ldots, 1)$ is monotone (trivially)
4. The limit of monotone vectors is monotone

## Why this matters

The conductance bound $\phi(P) \geq 1/(6L)$ relies on knowing that the stationary distribution $\pi_i = \alpha_i^2/Z$ is monotone. With monotonicity, any cut $B$ in the path has flow $\geq \pi_{k+1}/6$ across the boundary, and $\pi_{k+1} \geq 1/(2L)$ because the heavier side has at most $L$ terms that sum to $\geq 1/2$.

Without monotonicity, you'd need to know $\pi$ explicitly — which means knowing the ground state, which is what you're trying to analyze.

This is also used to bound the angle $\theta$ between the ground space of $H_{S_0}(s)$ and $M = \mathrm{diag}(1,0,\ldots,0)$ when applying Kitaev's geometric lemma (since $\cos\theta = \alpha_0 / \|\alpha\| \leq 1 - 1/L$ by monotonicity).

## When to use it

- Any setting where you've mapped a Hamiltonian to a random walk on a path/line via the [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping]]
- The ground state is unknown but the matrix $G$ visibly preserves monotonicity
- You need conductance bounds and can't compute $\pi$ directly

## Caveat

Requires the specific tridiagonal-with-non-negative-entries structure. For more general graphs or Hamiltonians with sign changes, monotonicity may not hold.

## Related notes

- [[Hamiltonian-to-Markov-Chain Spectral Gap Mapping]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[History-State Encoding with Unary Clock]]
