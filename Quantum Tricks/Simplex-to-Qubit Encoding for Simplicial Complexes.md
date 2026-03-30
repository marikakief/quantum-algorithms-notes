# Simplex-to-Qubit Encoding for Simplicial Complexes

> **Source:** Lloyd, Garnerone, Zanardi, arXiv:1408.3106 (2016)
> **Tags:** #trick #encoding #tda #simplicial-complex

## What it does
Maps an exponentially large simplicial complex ($2^n$ possible simplices on $n$ vertices) onto $n$ qubits, enabling quantum processing of topological data.

## The trick
Encode each simplex as a computational basis state: a $k$-simplex $\{v_0, v_1, \ldots, v_k\}$ becomes an $n$-bit string with 1s at positions $v_0, \ldots, v_k$. The subspace of Hamming weight $k+1$ in $(\mathbb{C}^2)^{\otimes n}$ corresponds to the space of potential $k$-simplices, with dimension $\binom{n}{k+1}$.

The simplicial complex at scale $\epsilon$ occupies a subspace $H_k^\epsilon$ of this Hamming-weight sector. The boundary operator $\partial_k$ maps between adjacent Hamming-weight sectors, and its action — removing one vertex with an alternating sign — looks exactly like a fermionic annihilation operator under the [[Fermionic-to-Qubit Mapping for Simplicial Dirac Operators|Jordan-Wigner mapping]].

Key property: the encoding is exponentially compressed ($n$ qubits for $2^n$ simplices) but the boundary operator remains $n$-sparse, enabling efficient [[Hamiltonian simulation]].

## When to reach for it
- Any quantum algorithm for algebraic topology on simplicial complexes
- When simplices are described by subsets of a vertex set (clique complexes, Vietoris-Rips complexes, Čech complexes)
- When you need quantum access to chain groups or homology computations

## Complexity
The encoding itself is free (it's just a labelling convention). The cost comes from preparing valid states in the subspace $H_k^\epsilon$ — typically $O(n^2 / \sqrt{\zeta_k})$ via [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]], where $\zeta_k$ is the clique fraction.

## Caveat
Only works when simplices are naturally described as subsets of vertices. Doesn't apply to abstract simplicial complexes given by facet lists (different input model). The encoding also requires a clique-checking oracle, which costs $O(k^2)$ per query if distances are in qRAM, or $O(|E|)$ if edges are stored directly (see [[Clique Checking via Edge Counting]]).

## Related notes
- [[Quantum Algorithms for Topological and Geometric Analysis of Data (Lloyd-Garnerone-Zanardi 2016) — Paper Notes]]
- [[Analyzing Prospects for Quantum Advantage in Topological Data Analysis (Berry, Su, Babbush et al 2024) — Paper Notes]]
- [[Quantum Computing and Persistence in TDA (Gyurik-Schmidhuber-King-Dunjko-Hayakawa 2024) — Paper Notes]] — Uses this encoding for the BQP$_1$-hardness reduction
- [[Subset States for Succinct Description of High-Dimensional Chains]] — Efficient classical descriptions of states in this encoding
- [[Fermionic-to-Qubit Mapping for Simplicial Dirac Operators]]
- [[Dicke State Preparation via Inequality Testing]]
