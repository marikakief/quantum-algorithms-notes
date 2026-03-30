# Projected Entangled Pairs as Variational Ansatz

> **Source:** Verstraete and Cirac, arXiv:cond-mat/0407066
> **Tags:** #trick #tensor-networks #PEPS #variational #area-law #2D-systems

## What it does
Provides a variational ansatz for quantum states on 2D (and higher-dimensional) lattices where the entanglement entropy of any block scales with its boundary area, not its volume.

## The trick
At each lattice site $(h,v)$, introduce auxiliary systems of dimension $D$ — one per lattice direction (four in 2D: up, down, left, right). Connect neighbouring auxiliary systems via maximally entangled pairs $|\phi\rangle = \sum_{n=1}^{D}|n,n\rangle$. Apply a local projector $Q_{h,v}$ at each site to map the auxiliary systems to the physical Hilbert space of dimension $d$.

The resulting state has amplitudes:

$$\langle s_{1,1},\ldots,s_{N_h,N_v}|\Psi\rangle = F_2(\{A^{s_{h,v}}_{h,v}\})$$

where $F_2$ contracts all bond indices according to the lattice geometry, and $(A^s_{h,v})_{u,d,l,r} = \langle s|Q_{h,v}|u,d,l,r\rangle$.

The entanglement entropy of any connected block is bounded by $b \log_2 D$, where $b$ is the number of bonds crossing the block's boundary. For a square block of side $L$ in 2D, $b \sim 4L$ — area-law scaling.

This is the natural generalisation of [[MPS Canonical Form for Efficient State Tracking|MPS]] from 1D chains (where each site has left/right bonds) to higher-dimensional lattices.

## When to reach for it
- Variational studies of 2D quantum lattice models (Heisenberg, Hubbard, frustrated magnets)
- Any setting where the target state obeys an area law for entanglement
- Describing ground states of gapped local Hamiltonians in any dimension — these generically obey area laws
- As a classical benchmark: if a PEPS calculation with modest $D$ matches your quantum algorithm's output, the quantum algorithm isn't doing anything a classical computer can't

## Complexity
- Parameters per site: $O(dD^4)$ in 2D (vs $O(dD^2)$ for MPS in 1D)
- Exact contraction of a 2D PEPS network is #P-hard
- Approximate contraction via [[Row-by-Row MPS Contraction for 2D Tensor Networks]] is polynomial in $N$ for fixed $D$

## Caveat
The approximate nature of PEPS contraction means you never get exact expectation values (unlike 1D MPS). The contraction error depends on the compression dimension $D_f$ and is hard to bound rigorously. Also, the cost scaling with $D$ is steep — $O(D^{10+})$ in practice for full variational optimisation — limiting practical bond dimensions to modest values.

Every state can be represented as a PEPS with sufficiently large $D$, but for states that violate the area law (e.g., highly excited states, or volume-law entangled states), $D$ would need to be exponentially large.

## Related notes
- [[Renormalization Algorithms for Quantum-Many Body Systems in Two and Higher Dimensions (Verstraete-Cirac 2004) — Paper Notes]]
- [[MPS Canonical Form for Efficient State Tracking]]
- [[Row-by-Row MPS Contraction for 2D Tensor Networks]]
- [[Variational Tensor Optimisation via Site-by-Site Sweeping]]
- [[Entanglement as Simulation Complexity Measure]]
