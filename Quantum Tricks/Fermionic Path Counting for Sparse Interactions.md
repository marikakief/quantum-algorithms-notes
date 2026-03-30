# Fermionic Path Counting for Sparse Interactions

> **Source:** Yuan Su, Hsin-Yuan Huang, Earl T. Campbell, arXiv:2012.09194  
> **Tags:** #trick #fermionic #combinatorics #trotter #hamiltonian-simulation #sparsity

## What it does

Bounds the [[Fermionic Seminorm for Trotter Error|fermionic seminorm]] of nested commutators by counting the number of nonzero computational-basis paths through a sparse interaction graph, giving $d$-dependent bounds instead of $n$-dependent ones.

## The trick

For a $d$-sparse interacting-electron Hamiltonian $H = T + V$ (at most $d$ nonzero entries per row/column of the coefficient matrices $\tau$ and $\nu$), bound

$$\|[H_{\gamma_{p+1}}, \ldots, [H_{\gamma_2}, H_{\gamma_1}]]\|_\eta$$

by expanding both the nested commutator and the $\eta$-electron states $|\psi_\eta\rangle = \sum_c \alpha_c |c\rangle$ in the computational (Fock) basis. Each term in the expansion corresponds to a sequence of creation/annihilation operations — a "path" through the modes.

The key insight: for $d$-sparse interactions, starting from any configuration $|c\rangle$, each layer of the commutator can only connect to $O(d)$ neighbouring configurations (since each hopping/interaction term involves at most $d$ non-zero couplings). So the total number of nonzero paths through $p+1$ commutator layers is $O(d^{p+1} \eta)$ rather than $O(n^{p+1} \eta)$. This gives:

$$\|S_p(t) - e^{-iHt}\|_\eta = O\!\left((\|\tau\|_{\max} + \|\nu\|_{\max})^{p-1} \|\tau\|_{\max} \|\nu\|_{\max} d^{p+1} \eta \, t^{p+1}\right)$$

## When to reach for it

- Lattice models with bounded coordination number (Hubbard, $t$-$J$, Heisenberg mapped to fermions)
- Any electronic Hamiltonian where the interaction graph is sparse ($d \ll n$) — e.g., local basis sets
- Comparing Trotter cost across different basis choices: plane-wave basis is not sparse ($d = n$), but localised bases can be

## Complexity

Analytical technique. The simulation cost is $O(n \cdot d^{1+1/p} \eta^{1/p})$ gates for the Fermi-Hubbard model (using locality of $e^{-itT}$ and $e^{-itV}$).

## Caveat

- For non-sparse interactions ($d = \Theta(n)$, e.g., plane-wave Coulomb), this reduces to the non-sparse bound and offers no improvement. Use the [[Recursive Operator Cauchy-Schwarz for Fermionic Bounds|recursive approach]] instead.
- The path-counting bound uses max-norms $\|\tau\|_{\max}$, $\|\nu\|_{\max}$ rather than spectral norms, so it can be looser than the recursive bound when the spectral norm is much smaller than $n \|\tau\|_{\max}$.
- The technique assumes computational-basis expansion, so it doesn't directly capture cancellations between paths (unlike the recursive approach).

## Related notes

- [[Nearly Tight Trotterization of Interacting Electrons (Su-Huang-Campbell 2021) — Paper Notes]]
- [[Fermionic Seminorm for Trotter Error]]
- [[Recursive Operator Cauchy-Schwarz for Fermionic Bounds]]
- [[Trotter Commutator-Scaling Bound]]
- [[Commutator-Sensitive Lieb-Robinson Bound]]
