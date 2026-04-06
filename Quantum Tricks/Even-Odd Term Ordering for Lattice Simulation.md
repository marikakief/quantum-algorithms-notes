# Even-Odd Term Ordering for Lattice Simulation

> **Source:** Andrew M. Childs and Yuan Su, arXiv:1901.00564
> **Tags:** #trick #hamiltonian-simulation #product-formulas #lattice #ordering

## What it does

Splits a nearest-neighbour lattice Hamiltonian into two commuting layers — odd bonds and even bonds — so each Trotter step is easy to implement and its error is controlled by only local overlaps.

## The trick

For a 1D lattice Hamiltonian

$$
H = \sum_{j=1}^{n-1} H_{j,j+1},
$$

define

$$
H_{\rm odd} = H_{1,2}+H_{3,4}+\cdots, \qquad H_{\rm even}=H_{2,3}+H_{4,5}+\cdots.
$$

Within each group, all terms commute because they act on disjoint pairs of qubits. So one Trotter step becomes

$$
S_1(t)=e^{-itH_{\rm even}}e^{-itH_{\rm odd}},
$$

and higher-order Suzuki formulas recurse on these two blocks.

The real payoff is error analysis:

$$
[H_{\rm even},H_{\rm odd}] = \sum_k [H_{2k-2,2k-1}+H_{2k,2k+1},\,H_{2k-1,2k}],
$$

because each bond only fails to commute with its two neighbouring bonds. So the first-order error is

$$
\|S_1(t)-e^{-iHt}\| = O(nt^2),
$$

not $O(n^2 t^2)$.

## Bonus: ordering robustness

For first-order formulas on a 1D lattice, **any** ordering of nearest-neighbour terms still has error $O(nt^2)$. You can transform an arbitrary ordering into even-odd ordering using at most $O(n)$ swaps of adjacent exponentials, and each swap costs only $O(t^2)$ when the terms overlap.

So even-odd ordering is not the only asymptotically valid choice, but it is the cleanest one for proofs and implementations.

## When to reach for it

- Simulating 1D nearest-neighbour spin chains
- Building product-formula circuits with shallow commuting layers
- Proving locality-sensitive Trotter bounds

## Generalisations

- Periodic boundary conditions: use three commuting groups
- Range-$\ell$ interactions: use $\ell$ commuting groups
- $D$-dimensional lattices: use a $2D$ edge colouring

## Related notes

- [[Nearly Optimal Lattice Simulation by Product Formulas (Childs-Su 2019) — Paper Notes]]
- [[Local Error Representation for Lattice Product Formulas]]
- [[Product-Formula Time-Slicing for Local Hamiltonians]]
- [[Product Formulas]]
