# Jagged Adiabatic Path via Projections

> **Source:** Aharonov & Ta-Shma, arXiv:quant-ph/0301023 (2003), Lemma 2
> **Tags:** #trick #adiabatic #spectral-gap #state-generation

## What it does

Connects a sequence of Hamiltonians by an adiabatic path with guaranteed inverse-polynomial spectral gap, even when the naive straight-line interpolation would close the gap.

## The trick

Given simulatable Hamiltonians $\{H_j\}_{j=1}^T$ with ground states $|\alpha(H_j)\rangle$ satisfying $|\langle\alpha(H_j)|\alpha(H_{j+1})\rangle| \geq 1/\mathrm{poly}(n)$:

1. **Replace each $H_j$ by its ground-state projector:** $\Pi_{H_j} = I - |\alpha(H_j)\rangle\langle\alpha(H_j)|$. These have gap exactly 1. Implement via [[Hamiltonian-to-Projection via Phase Estimation]].

2. **Connect adjacent projectors by straight lines:** The convex combination $H_\eta = (1-\eta)\Pi_{H_j} + \eta\Pi_{H_{j+1}}$ has gap $\geq |\langle\alpha(H_j)|\alpha(H_{j+1})\rangle|$ for all $\eta \in [0,1]$.

3. **Apply adiabatic evolution** along the resulting "jagged" path. All gaps are $\geq 1/\mathrm{poly}(n)$, so total time is $\mathrm{poly}(n)$.

```
 Naive straight line: gap can vanish
 ────────────×────────────
              ↑ gap = 0

 Jagged path via projections: gap guaranteed
 ─Π₁──────Π₂──────Π₃──────Π₄─
   \  ≥|⟨α₁|α₂⟩| /  ≥|⟨α₂|α₃⟩| /
```

## Why the gap bound works

Write $|\beta\rangle = a|\alpha\rangle + b|\alpha^\perp\rangle$. In the $\{|\alpha\rangle, |\alpha^\perp\rangle\}$ basis, $H_\eta$ has a 2D block with gap $\sqrt{1 - 4(1-\eta)\eta|b|^2}$, minimized at $\eta = 1/2$ where it equals $|a| = |\langle\alpha|\beta\rangle|$.

## When to reach for it

- Designing adiabatic state generation algorithms
- Any situation where you have a chain of Hamiltonians with overlapping ground states and need to morph between them
- Constructing spectral-gap-preserving paths for adiabatic computation

## Caveat

Requires implementing the projectors $\Pi_{H_j}$ via [[Hamiltonian-to-Projection via Phase Estimation|phase estimation]], which adds overhead proportional to $1/\Delta(H_j)$. If the original spectral gaps are already tiny, the projection step is expensive.

## Related notes

- [[Adiabatic Quantum State Generation and Statistical Zero Knowledge (Aharonov-Ta-Shma 2003) — Paper Notes]]
- [[Hamiltonian-to-Projection via Phase Estimation]]
- [[Adiabatic Quantum Computation is Equivalent to Standard Quantum Computation (Aharonov-van Dam-Kempe-Landau-Lloyd-Regev 2004) — Paper Notes]]
- [[Gapped Phase Estimation]]
