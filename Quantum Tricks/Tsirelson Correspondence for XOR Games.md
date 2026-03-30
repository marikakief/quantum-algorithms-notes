# Tsirelson Correspondence for XOR Games

> **Source:** Cleve, Høyer, Toner, Watrous, arXiv:quant-ph/0404076 (presentation); Tsirelson (1987) (original theorem)
> **Tags:** #trick #nonlocal-games #semidefinite-programming #bell-inequality #XOR-games

## What it does
Reduces the quantum value of any XOR game to a semidefinite program over real unit vectors, making it efficiently computable despite the infinite-dimensional optimisation over quantum strategies.

## The trick

**Tsirelson's theorem (Theorem 4):** For finite sets $S, T$ and reals $c_{s,t} \in [-1,1]$, the following are equivalent:

1. There exist dimension $n$, state $|\psi\rangle \in \mathbb{C}^n \otimes \mathbb{C}^n$, and $\pm 1$ observables $\{A_s\}$, $\{B_t\}$ such that $\langle\psi| A_s \otimes B_t |\psi\rangle = c_{s,t}$

2. There exist real unit vectors $\{|u_s\rangle\}$, $\{|v_t\rangle\}$ in $\mathbb{R}^m$ such that $\langle u_s | v_t \rangle = c_{s,t}$

For an XOR game $G$, the quantum value becomes:

$$\omega_q(G) = \tau(G) + \frac{1}{2} \max_{\{|u_s\rangle\}, \{|v_t\rangle\}} \sum_{s,t} \pi(s,t)(V(0|s,t) - V(1|s,t))\langle u_s | v_t \rangle$$

where $\tau(G)$ is the trivial random strategy value. This is a semidefinite program, solvable in polynomial time to arbitrary precision.

The classical value is the same optimisation restricted to $|u_s\rangle, |v_t\rangle \in \{-1, +1\}$ — an integer relaxation. The gap is bounded by Grothendieck's constant: $\omega_q - \tau \leq K_G(\omega_c - \tau)$ with $K_G \approx 1.78$.

## When to reach for it

- Computing quantum values of XOR games exactly (or to arbitrary precision via SDP)
- Proving Tsirelson-type bounds on Bell inequality violations
- Connecting quantum correlations to the Goemans-Williamson MAX-CUT framework (same rounding technique)
- Bounding entanglement needed for near-optimal strategies (via Johnson-Lindenstrauss)

## Complexity

$\omega_q$ computable in $\mathrm{poly}(|S| + |T|, \log(1/\varepsilon))$ for XOR games. The classical value $\omega_c$ is NP-hard (integer quadratic program).

Entanglement: optimal strategy needs at most $\lceil m/2 \rceil$ EPR pairs where $m = \min(|S|, |T|)$. Near-optimal ($\varepsilon$-close) needs $O(\log(|S|+|T|)/\varepsilon^2)$ qubits via Johnson-Lindenstrauss.

## Caveat

Applies only to XOR games (predicate depends on $a \oplus b$ only). For general binary games, let alone games with larger answer sets, no such clean SDP characterisation exists. General nonlocal game value computation is undecidable.

## Related notes
- [[Consequences and Limits of Nonlocal Strategies (Cleve-Hoyer-Toner-Watrous 2004) — Paper Notes]]
- [[Nonlocal Game Framework]]
- [[Magic Square Pseudo-Telepathy]]
