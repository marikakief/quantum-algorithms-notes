# Nonlocal Game Framework

> **Source:** Cleve, Høyer, Toner, Watrous, arXiv:quant-ph/0404076
> **Tags:** #trick #nonlocal-games #bell-inequality #interactive-proofs #entanglement

## What it does
Provides a clean computational abstraction for any scenario where two non-communicating parties produce correlated outputs, with separate classical and quantum values measuring the power of shared entanglement.

## The trick

A **nonlocal game** $G = G(V, \pi)$ is defined by:
- Finite question sets $S, T$ and answer sets $A, B$
- A distribution $\pi$ on $S \times T$ (the referee's question distribution)
- A predicate $V(a, b \mid s, t)$ (win/lose condition)

**Classical value:** $\omega_c(G) = \max_{a(\cdot), b(\cdot)} \sum_{s,t} \pi(s,t) V(a(s), b(t) \mid s, t)$

Any probabilistic strategy is a convex combination of deterministic strategies, so the maximum over deterministic strategies suffices.

**Quantum value:** $\omega_q(G) = \sup \sum_{s,t} \pi(s,t) \sum_{a,b} \langle\psi| X^a_s \otimes Y^b_t |\psi\rangle V(a, b \mid s, t)$

where the supremum is over all dimensions $n$, states $|\psi\rangle \in \mathbb{C}^n \otimes \mathbb{C}^n$, and POVMs $\{X^a_s\}$, $\{Y^b_t\}$.

The gap $\omega_q(G) - \omega_c(G)$ is precisely the "Bell inequality violation" in game-theoretic language.

**Key structural results:**
- For binary games: $\omega_q = 1 \Rightarrow \omega_c = 1$ (projective measurements suffice; Proposition 2)
- For XOR games ($V$ depends only on $a \oplus b$): $\omega_q$ is computable via SDP (Tsirelson's theorem)
- For general games: computing $\omega_q$ is undecidable (consequence of MIP* = RE)

## When to reach for it

- Analysing device-independent protocols (QKD, randomness certification) — security reduces to bounding $\omega_q$
- Studying multi-prover interactive proof systems (MIP, MIP*) — the soundness condition is an upper bound on $\omega_q$
- Connecting physics (Bell inequalities, contextuality) with computation (proof systems, communication complexity)
- Proving impossibility results for classical simulation of quantum correlations

## Complexity

Computing $\omega_c$: NP-hard for general games (even for XOR games, the integer optimisation is NP-hard).
Computing $\omega_q$: Polynomial for XOR games (SDP). Undecidable for general games.

## Caveat

$\omega_q$ is a supremum, not necessarily a maximum — there may be no finite-dimensional strategy achieving it. This is not a technicality: the gap between the finite-dimensional quantum value and the commuting-operator value is at the heart of MIP* = RE and Connes' embedding problem.

## Related notes
- [[Consequences and Limits of Nonlocal Strategies (Cleve-Hoyer-Toner-Watrous 2004) — Paper Notes]]
- [[Tsirelson Correspondence for XOR Games]]
- [[Magic Square Pseudo-Telepathy]]
- [[Quantum Arthur-Merlin Games (Marriott-Watrous 2005) — Paper Notes]]
