# Ordering Robustness for First-Order Lattice Trotter

> **Source:** Andrew M. Childs and Yuan Su, arXiv:1901.00564
> **Tags:** #trick #hamiltonian-simulation #product-formulas #lattice #ordering

## What it does

For a first-order [[Product Formulas|product formula]] on a 1D nearest-neighbour lattice, **any** ordering of the exponentials gives the same $O(nt^2)$ error bound. You do not need to care which permutation you use.

## The trick

Fix any permutation $\sigma \in S_{n-1}$ of the $n-1$ nearest-neighbour terms and consider the first-order formula

$$
\prod_{j=1}^{n-1} e^{-it H_{\sigma(j),\sigma(j)+1}}.
$$

Bound its error by splitting into two parts:

$$
\left\|\prod_j e^{-itH_{\sigma(j),\sigma(j)+1}} - e^{-itH}\right\| \leq \underbrace{\left\|\prod_j e^{-itH_{\sigma(j),\sigma(j)+1}} - e^{-itH_{\rm even}}e^{-itH_{\rm odd}}\right\|}_{\text{reordering cost}} + \underbrace{\left\|e^{-itH_{\rm even}}e^{-itH_{\rm odd}} - e^{-itH}\right\|}_{O(nt^2)}.$$

For the reordering cost: transform the arbitrary ordering into even-odd ordering by a sequence of adjacent swaps. The key estimate is

$$
\left\|\left[e^{-itH_{k,k+1}},\, e^{-itH_{l,l+1}}\right]\right\| \leq 2t^2
$$

if $|k - l| = 1$ (overlapping supports), and $0$ otherwise (disjoint supports commute exactly). At most $2n$ such swaps are needed (bubble-sort to even-odd), each costing $O(t^2)$, giving reordering error $O(nt^2)$. Combined with the even-odd error, the total is still $O(nt^2)$.

## When to reach for it

- You are implementing a first-order Trotter circuit and term ordering is determined by hardware constraints (e.g., connectivity, parallelism).
- You want to argue that an experimentally convenient ordering is asymptotically just as good as the textbook even-odd split.
- You need to compare different orderings: the asymptotic bound is universal, but constants differ — the even-odd ordering has smaller empirical prefactor in practice.

## Complexity

No additional gates beyond the standard first-order Trotter circuit. The reordering argument is purely in the error analysis. The $O(nt^2)$ per-step bound is the same regardless of ordering; to achieve accuracy $\varepsilon$ you still need $r = O(nt^2/\varepsilon)$ segments with $O(n)$ gates each, for total gate count $O((nt)^2/\varepsilon)$.

## Caveat

This applies to **first-order formulas only**. Whether ordering robustness extends to higher-order formulas is open — for $k \geq 2$ the argument does not go through because swapping exponentials in a higher-order formula changes the underlying algebraic cancellations. In practice, even-odd ordering empirically outperforms the X-Y-Z ordering for both first- and fourth-order formulas (Figure 2 of the supplementary).

## Related notes

- [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]]
- [[Even-Odd Term Ordering for Lattice Simulation]]
- [[Local Error Representation for Lattice Product Formulas]]
- [[Product Formulas]]
