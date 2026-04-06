# Frobenius-Norm Commutator Sum for Product Formulas

> **Source:** Zhao, Zhou, Shaw, Li, Childs, arXiv:2111.04773
> **Tags:** #trick #trotter #product-formulas #average-case #frobenius-norm

## What it does

Replaces the spectral-norm commutator sum $\alpha_{\text{comm},p}$ from [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes|Childs et al. (2019)]] with its Frobenius-norm analogue $T_p$, giving tighter average-case [[Product Formulas|product formula]] error bounds for local Hamiltonians.

## The trick

The worst-case $p$-th order product formula error is controlled by:

$$
\alpha_{\text{comm},p} = \sum_{l_1, \ldots, l_{p+1}} \|[H_{l_1}, [H_{l_2}, \ldots, [H_{l_p}, H_{l_{p+1}}]\cdots]]\|.
$$

For average-case error over 1-design inputs, replace each spectral norm with $\frac{1}{\sqrt{d}}$ times the Frobenius norm:

$$
T_p = \sum_{l_1, \ldots, l_{p+1}} \frac{1}{\sqrt{d}} \|[H_{l_1}, [H_{l_2}, \ldots, [H_{l_p}, H_{l_{p+1}}]\cdots]]\|_F.
$$

The average error is then $R = O(T_p \, t^{p+1} / r^p)$.

For the triangle bounds (PF1 and PF2), some summations can be moved inside the Frobenius norm for tighter constants:

$$
T_1' = \frac{1}{2\sqrt{d}} \sum_{l_1=1}^{L-1} \left\|\left[H_{l_1}, \sum_{l_2 > l_1} H_{l_2}\right]\right\|_F.
$$

**Why the Frobenius norm helps:** For a nested commutator of local operators on $n$ qubits, the spectral norm captures the largest eigenvalue while the Frobenius norm captures the root-mean-square eigenvalue. When the commutator is supported on a small number of sites, most eigenvalues are zero (identity on the rest of the system). The spectral norm doesn't benefit from this, but $\frac{1}{\sqrt{d}} \|A\|_F$ does — it effectively divides by $\sqrt{d}$ and gains back only $\sqrt{2^k}$ where $k$ is the support size.

For nearest-neighbor chains: $\alpha_{\text{comm},p} = O(n)$ but $T_p = O(\sqrt{n})$, because each nested commutator is local and the sum over $n$ terms gains a $\sqrt{n}$ factor via the Frobenius norm rather than $n$ via the spectral norm.

## When to reach for it

- Any time you're bounding [[Product Formulas|product formula]] errors and the input state is not worst-case (1-design, random product state, computational basis average, etc.)
- Particularly effective for geometrically local Hamiltonians where commutators have bounded support
- OTOC error analysis where one subsystem is maximally mixed

## Complexity

No computational overhead. The bounds apply to the same product formula circuits — only the analysis changes.

## Caveat

- Only gives average-case guarantees. Not useful when you need worst-case bounds (e.g., fault-tolerant error budgets).
- The $\sqrt{n}$ improvement for nearest-neighbor chains is the best case. For $k$-local Hamiltonians the improvement is $n^{(k-1)/2}$ in the exponent, which matters less for small $k$.
- For power-law interactions with $\alpha > D$ (rapidly decaying), the average and worst case have the same $n$-scaling.

## Related notes

- [[Hamiltonian Simulation with Random Inputs (Zhao-Zhou-Shaw-Li-Childs 2022) — Paper Notes]]
- [[Frobenius-Norm Average-Case Error Bound]]
- [[Trotter Commutator-Scaling Bound]]
- [[A Theory of Trotter Error (Childs-Su-Tran-Wiebe-Zhu 2019) — Paper Notes]]
- [[Destructive Error Interference in Product-Formula Lattice Simulation (Tran-Chu-Su-Childs-Gorshkov 2020) — Paper Notes]]
