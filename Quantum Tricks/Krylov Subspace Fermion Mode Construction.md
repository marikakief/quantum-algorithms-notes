# Krylov Subspace Fermion Mode Construction

> **Source:** Chapman, Elman, Mann, arXiv:2305.15625
> **Tags:** #trick #free-fermion #Krylov-subspace #operator-algebra

## What it does
Constructs physical fermion modes by iterating the commutator $\mathrm{ad}_{iH} = [iH, \cdot\,]$ on a seed operator, generating a finite-dimensional operator Krylov subspace whose structure reveals the single-particle Hamiltonian.

## The trick
Choose a Pauli operator $\chi = \sigma_{j^*}$ not in the Hamiltonian, anticommuting only with the operators in a simplicial clique of the frustration graph. Then compute:

$$\mathrm{ad}^0_{iH}\chi = \chi, \quad \mathrm{ad}^1_{iH}\chi = [iH, \chi], \quad \mathrm{ad}^k_{iH}\chi = [iH, \mathrm{ad}^{k-1}_{iH}\chi]$$

Within each cycle-symmetry sector $\mathcal{J}$, these operators become linearly dependent at rank $2\alpha(G)$ (twice the independence number). The matrix

$$\Pi_{\mathcal{J}}\{\mathrm{ad}^j_{iH}\chi, \mathrm{ad}^k_{iH}\chi\} = 2(M_{G,\mathcal{J}})_{jk}\,\Pi_{\mathcal{J}}$$

is positive definite and plays the role of a **generalised canonical anticommutation relation**. Diagonalising $M_{G,\mathcal{J}}$ yields the physical Majorana modes as linear combinations of $\{\mathrm{ad}^k_{iH}\chi\}_k$.

The effective single-particle Hamiltonian is the weighted adjacency matrix $A_{G,\mathcal{J}}$ of a directed bipartite graph derived from the frustration graph.

## When to reach for it
- Extracting explicit fermion modes from a Hamiltonian known to be free-fermion solvable
- When the Jordan-Wigner transformation doesn't apply (frustration graph isn't a line graph) but the SCF criterion is satisfied
- Connecting free-fermion physics to operator Krylov subspace methods

## Complexity
Computing $\mathrm{ad}^k_{iH}\chi$ for $k = 0, \ldots, 2\alpha(G)-1$ costs $O(\alpha(G) \cdot |V|)$ operator multiplications. The Krylov closure guarantees this terminates.

## Caveat
The Krylov space only closes at finite rank because of the cycle symmetries. Without them (e.g., for interacting models), the Krylov space may grow exponentially. The construction is specific to free-fermion solvable models.

## Related notes
- [[A Unified Graph-Theoretic Framework for Free-Fermion Solvability (Chapman-Elman-Mann 2023) — Paper Notes]]
- [[Frustration Graph Criterion for Free-Fermion Solvability]]
- [[Generalized Cycle Symmetries from Even Holes]]
