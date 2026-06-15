# Twisted Bent Target Sets for OPI Hardness

> **Source:** Khattar, Shutty, Gidney, Zalcman, Yosri, Maslov, Babbush, Jordan, arXiv:2510.10967
> **Tags:** #trick #OPI #finite-fields #bent-functions #classical-hardness #DQI

## What it does

Builds OPI target sets from random linear images of a Maiorana-McFarland bent set so that affine-subspace attacks have limited overlap.

## The trick

Use the bent set

$$
S_k =
\left\{(x,y)\in \mathbb{F}_2^k\times\mathbb{F}_2^k :
\langle x,y\rangle = 1
\right\}.
$$

For each OPI evaluation point $\alpha$, choose an independent random invertible matrix $A_\alpha \in GL_{2k}(\mathbb{F}_2)$ and set

$$
F_\alpha = \phi^{-1}(A_\alpha \phi(S_k)).
$$

The random linear twist changes the target set per coordinate while preserving efficient quantum implementation: membership phase for $S_k$ is a layer of CZ gates, and the twist is a reversible linear basis change.

The key bound is that for any affine subspace $A\subseteq \mathbb{F}_2^{2k}$ of dimension $d$,

$$
|A \cap S_k| \le
\begin{cases}
2^d & d < k,\\
2^{d-1}+2^{k-2} & k \le d < 2k-1,\\
2^{2k-2} & d = 2k-1,\\
2^{2k-1}-2^{k-1} & d = 2k.
\end{cases}
$$

This limits how much [[Extended Prange Attack over Extension Fields]] can gain by restricting coordinates to affine subspaces.

## When to reach for it

- You need OPI target sets that are cheap for DQI objective oracles but awkward for extension-field Prange attacks.
- You want target sets with provable affine-intersection control.
- You need a structured family for asymptotic DQI advantage arguments.

## Complexity

The target phase can be implemented with one transversal CZ layer for $S_k$ plus an $O(k^2)$ CNOT basis change for the random linear twist. In the paper's asymptotic theorem, this satisfies the $\widetilde{O}(1)$ per-constraint objective-oracle condition when $b=2k=O(\mathrm{polylog}(m))$.

## Caveat

The construction is tuned to the XP attack model. It does not rule out new attacks that use the polynomial structure of OPI more directly. The authors also state that choosing optimal target sets is still open.

## Related notes

- [[Verifiable Quantum Advantage via Optimized DQI Circuits (Khattar, Shutty, Gidney et al 2025) — Paper Notes]]
- [[Extended Prange Attack over Extension Fields]]
- [[Optimization by Decoded Quantum Interferometry (Jordan, Shutty, Wootters, Babbush et al 2024) — Paper Notes]]
