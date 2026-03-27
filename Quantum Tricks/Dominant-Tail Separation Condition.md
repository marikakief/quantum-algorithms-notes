
> **Source:** Ding, Li, Lin, Ni, Ying, Zhang, arXiv:2402.01013
> **Tags:** #trick #spectral-estimation #initial-state #overlap #phase-estimation

## What it does

Replaces the traditional spectral gap assumption in eigenvalue estimation with a condition purely on initial-state overlaps, enabling Heisenberg-limited estimation even when eigenvalues are arbitrarily close.

## The trick

Partition the spectrum into *dominant* eigenvalues (index set $D$) and the rest. Define:

$$p_{\min} := \min_{i \in D} p_i, \qquad p_{\text{tail}} := \sum_{i \notin D} p_i$$

where $p_i = |\langle \psi_i | \psi \rangle|^2$ is the overlap of the initial state with the $i$-th eigenstate.

The **sufficiently dominant condition** requires $p_{\min} > p_{\text{tail}}$: each dominant eigenvalue individually has more overlap than the entire tail combined.

This is weaker than a spectral gap assumption because it says nothing about where the eigenvalues are — only about how much the initial state "sees" them. Two eigenvalues can be $10^{-15}$ apart and both be dominant, as long as their overlaps exceed the tail weight. The estimation precision is then controlled by $T$ (evolution time), not by the eigenvalue separation.

The sample complexity scales as $(p_{\min} - p_{\text{tail}})^{-2}$, so the margin between dominant overlaps and tail weight directly controls the cost.

## When to reach for it

- Multiple eigenvalue estimation when you have a good initial state but no spectral gap guarantee.
- Problems where eigenvalue spacing is unknown or arbitrarily small (e.g., highly degenerate or near-degenerate systems).
- As a conceptual replacement for "initial overlap $\geq \gamma$" conditions in single-eigenvalue algorithms — it generalises naturally to multiple eigenvalues.

## Complexity

No direct quantum cost — this is an assumption that controls the sample complexity of [[Gaussian Time-Sampling Filter for Spectral Estimation|Gaussian-filtered]] spectral estimation. The cost to achieve $\epsilon$-precision scales as $T_{\text{tot}} = \tilde{O}((p_{\min} - p_{\text{tail}})^{-2} / \epsilon)$.

## Caveat

The condition is *not* checkable a priori in general — you need to know (or assume) something about the overlaps of your initial state. In practice, this means you need an initial state that's genuinely biased toward the eigenvalues you care about. If the tail weight dominates (e.g., a random initial state for a large Hamiltonian), the condition fails and the algorithm provides no guarantees. This is a fundamental limitation, not a technical one — no algorithm can estimate eigenvalues with negligible overlap.

## Related notes
- [[Quantum Multiple Eigenvalue Gaussian Filtered Search (Ding-Li-Lin-Ni-Ying-Zhang 2024) — Paper Notes]]
- [[Gaussian Time-Sampling Filter for Spectral Estimation]]
- [[Greedy Peak Search with Blocking]]
- [[Approximate CDF from Hadamard Tests]]
- [[Heisenberg-Limited Ground-State Energy Estimation for Early Fault-Tolerant QC (Lin-Tong 2022) — Paper Notes]]
