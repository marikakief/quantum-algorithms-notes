# Recursive Operator Cauchy-Schwarz for Fermionic Bounds

> **Source:** Yuan Su, Hsin-Yuan Huang, Earl T. Campbell, arXiv:2012.09194  
> **Tags:** #trick #fermionic #operator-inequality #trotter #hamiltonian-simulation

## What it does

Bounds the [[Fermionic Seminorm for Trotter Error|fermionic seminorm]] of products of creation, annihilation, and number operators by recursively contracting indices via operator inequalities, converting $n$-dependent norms into $\eta$-dependent ones.

## The trick

Given a fermionic operator $X = \sum_{j,k,l,\ldots} w_{j,k,l,\ldots} \cdots A_j^\dagger \cdots N_l \cdots A_k \cdots$, bound $\|X\|_\eta$ by:

1. **Rewrite** as $\|X\|_\eta = \max_{|\psi_\eta\rangle} \sqrt{\langle \psi_\eta | X^\dagger X | \psi_\eta \rangle}$
2. **Contract one index pair** in $X^\dagger X$ using either:
   - *Operator Cauchy-Schwarz:* $\pm \sum_{j,k} B_j^\dagger C_k^\dagger C_j B_k \leq \sum_{j,k} B_j^\dagger C_k^\dagger C_k B_j$ (PSD upper bound)
   - *Diagonalisation:* $\sum_{j,k} \mu_{j,k} B_j^\dagger B_k \leq \|\mu\| \sum_j B_j^\dagger B_j$ when $\mu$ is Hermitian
3. **Hölder-type factorisation:** $\|\sum_j B_j^\dagger C_j^\dagger C_j B_j\|_\eta \leq \|\sum_j B_j^\dagger B_j\|_\eta \cdot \max_k \|C_k^\dagger C_k\|_\xi$
4. **Recurse** until all indices are contracted

Each contraction step replaces a sum over spin-orbital indices (contributing a factor of $n$ or $\|\tau\|$) with a term involving the particle number $\eta$ or the spectral norm $\|\tau\|$. For the single-layer commutator $[T, V]$:

$$\|[T,V]\|_\eta \leq O(\|\tau\| \cdot \|\nu\|_{\max} \cdot \eta^2)$$

where the $\eta^2$ comes from the two number operators $N_l, N_m$ in $V$ each contributing at most $\eta$ within the $\eta$-electron sector. Compare with the spectral norm bound, which would give $O(\|\tau\| \cdot \|\nu\| \cdot n^2)$ — potentially much larger.

## When to reach for it

- Bounding expectations of complex products of fermionic operators within a fixed particle sector
- Trotter error analysis for any Hamiltonian with the structure $T + V$ where $T$ is quadratic (hopping) and $V$ is diagonal in number operators
- More generally, any situation where you need tight operator norm bounds that respect the algebra of creation/annihilation operators

## Complexity

This is an analytical technique, not an algorithm. No computational cost.

## Caveat

- The recursive application introduces multiplicative factors at each level; for very deep nesting (high-order [[Product Formulas]]), the accumulated prefactors can become loose
- The technique is specific to the structure of fermionic operators — it doesn't generalise to arbitrary operator algebras
- The Hölder-type inequality (Step 3) requires $B_j$ to map between fixed particle sectors and $C_j$ to be number-preserving

## Related notes

- [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes]]
- [[Fermionic Seminorm for Trotter Error]]
- [[Trotter Commutator-Scaling Bound]]
- [[Finite Nested-Commutator Expansion]]
