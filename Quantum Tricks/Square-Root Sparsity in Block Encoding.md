
> **Source:** Babbush, Berry, Kothari, Somma, Wiebe, arXiv:2303.13012
> **Tags:** #trick #block-encoding #sparsity #Hamiltonian-simulation #query-complexity

## What it does
Achieves $O(\sqrt{d})$ dependence on sparsity in the block encoding normalization factor, instead of the usual $O(d)$.

## The trick

Standard sparse [[Hamiltonian simulation]] block-encodes a $d$-sparse matrix $A$ with normalization $\Lambda = O(\|A\|_{\max} d)$. This comes from a matched pair of state preparations (PREPARE and PREPARE$^\dagger$), each contributing $\sqrt{d}$.

When $A = BB^\dagger$ and you simulate via the block Hamiltonian $H = \begin{pmatrix} 0 & B \\ B^\dagger & 0 \end{pmatrix}$, the block encoding of $B$ requires only a *single* sparse-state preparation: you prepare $\frac{1}{\sqrt{d}} \sum_\ell |\ell\rangle$ once, then query the oracle for column positions and values. No matched inverse preparation is needed.

Result: $\Lambda = \sqrt{2\aleph d}$ where $\aleph = \kappa_{\max}/m_{\min}$, giving query complexity $O(t\sqrt{\aleph d} + \log(1/\varepsilon))$ — sublinear in $d$.

**Intuition:** Simulating $H$ (the "square root" of $A$) reduces the effective sparsity because $H$'s structure is asymmetric (rectangular blocks), requiring only one-sided access.

## When to reach for it

- The Hamiltonian has a natural factored form $A = BB^\dagger$ (graph Laplacians, discretized differential operators, Lindbladian components).
- Sparsity $d$ is large enough that the $\sqrt{d}$ vs $d$ distinction matters.
- You're simulating evolution under $H$ (effectively $\sqrt{A}$), not $A$ directly.

## Complexity

For the coupled oscillator problem: $O(t\sqrt{\kappa_{\max} d / m_{\min}} + \log(1/\varepsilon))$ queries, vs $O(t \cdot \kappa_{\max} d / m_{\min})$ for naive block encoding of $A$.

## Caveat

Only applies when you have the factored form $A = BB^\dagger$ with $B$ sparse. If $A$ is given directly and the factorization is expensive or non-sparse, you lose the advantage.

## Related notes
- [[Exponential Quantum Speedup in Simulating Coupled Classical Oscillators (Babbush, Berry, Kothari, Somma, Wiebe 2023) — Paper Notes]]
- [[Incidence Matrix Factorization for Square-Root Block Encoding]]
- [[Qubitization (Quantum Walk for Spectral Encoding)]]
