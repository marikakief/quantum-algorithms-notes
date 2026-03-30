# Subset States for Succinct Description of High-Dimensional Chains

> **Source:** Gyurik, Schmidhuber, King, Dunjko, Hayakawa, arXiv:2410.21258 (2024)
> **Tags:** #trick #encoding #tda #simplicial-complex #subset-state

## What it does

Provides an efficient classical description of a quantum state corresponding to an exponentially large set of high-dimensional simplices, enabling polynomial-sized inputs for problems on exponentially large chain spaces.

## The trick

Consider a clique complex $K = \text{Cl}(G)$ where $G = G_1 * G_2 * \cdots * G_p$ is a graph join. A $(2p-1)$-simplex in $K$ corresponds to a product of edges, one from each component graph. Define:

$$|S\rangle = \bigotimes_{i=1}^{p} \left( \frac{1}{\sqrt{|E^{(i)}|}} \sum_{e \in E^{(i)}} |e\rangle \right)$$

where each $E^{(i)}$ is a set of edges from $G_i$. The state $|S\rangle$ is a uniform superposition over $|S| = \prod_i |E^{(i)}|$ simplices — potentially exponentially many — but is fully specified by listing the $p$ edge sets $\{E^{(i)}\}$, which is polynomial in the number of vertices.

For unions of such product sets $S = \bigcup_{i=1}^m S_i$, the subset state is:

$$|S\rangle = \frac{1}{\sqrt{m}} \sum_{i=1}^m |S_i\rangle$$

described by $m \times p$ edge sets total.

The [[Simplex-to-Qubit Encoding for Simplicial Complexes|simplex-to-qubit encoding]] maps each edge in $G_i$ to a 1-cycle in the $i$-th qubit gadget graph (left cycle for $|0\rangle$, right cycle for $|1\rangle$), so computational basis states of $n$ qubits map to tensor products of 1-cycles — exactly the product structure needed.

## When to reach for it

- Specifying inputs for TDA problems on high-dimensional simplicial complexes (where chains live in exponentially large spaces)
- Any quantum algorithm that needs a succinct classical description of a superposition over an exponentially large set with product structure
- Complexity reductions where the "input state" must be classically describable in polynomial size (as in the Harmonic Persistence problem)

## Complexity

The description size is $O(m \cdot p \cdot |V|)$ where $m$ is the number of product components, $p$ is the join depth, and $|V|$ is the vertex count. Preparing the corresponding quantum state costs $O(m \cdot p \cdot |V|)$ gates (tensor product of uniform superpositions).

## Caveat

Only works for states with the specific product-of-uniform-superpositions structure. General harmonic representatives of holes need not be subset states. The reduction in the paper specifically constructs instances where the relevant harmonics have this form (via the [[Pre-History State as Guided Sparse Hamiltonian Input|pre-history state]] construction).

## Related notes
- [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]]
- [[Simplex-to-Qubit Encoding for Simplicial Complexes]]
- [[Pre-History State as Guided Sparse Hamiltonian Input]]
