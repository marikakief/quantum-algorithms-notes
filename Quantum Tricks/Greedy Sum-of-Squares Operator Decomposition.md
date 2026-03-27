
> **Source:** Rubin, Lee, Babbush, arXiv:2109.05010 (2021)
> **Tags:** #trick #operator-decomposition #circuit-compilation #classical-preprocessing #fermionic-Gaussian

## What it does

Decomposes a generic antihermitian two-body fermion operator into a sum of squared normal one-body operators, minimizing the number of terms needed for a given accuracy. Each term maps directly to a quantum circuit layer of [[Givens Rotation Slater Determinant Preparation|Givens rotations]] + [[Fermionic Swap Network|Ising interactions]].

## The trick

Given a two-body operator $G = \sum_{pqrs} A^{pq}_{rs} a^\dagger_p a^\dagger_q a_s a_r$, we want

$$G \approx \sum_{l=1}^{L} Z_l^2, \quad Z_l = \sum_{pq} z^{(l)}_{pq} a^\dagger_p a_q$$

with $L$ as small as possible. The algorithm is greedy:

1. **Optimize:** Find the single-particle rotation $U(\kappa) = e^{\sum_{pq} \kappa_{pq} a^\dagger_p a_q}$ that maximizes the $n_i n_j$ content of the rotated operator:
   $$\max_\kappa \sum_{xy} |\tilde{t}_{xx,yy}(\kappa)|^2$$
   using gradient descent. The gradient costs $O(n^5)$ via the chain rule through the unitary parameterization, using the Wilcox identity for $\partial u / \partial \kappa_{ab}$.

2. **Extract:** Diagonalize the optimized one-body operator to get rotation $U_l$ and coupling matrix $J^{(l)}_{pq}$.

3. **Subtract:** Transform the extracted component back to the original basis and subtract from the residual tensor.

4. **Repeat** until $\|A_{\text{residual}}\|$ falls below threshold.

The $J^{(l)}$ matrices produced are full-rank (not restricted to rank 1 as in SVD/Takagi decompositions), so each factor carries more information. This is the main source of the compression advantage — see [[Full-Rank Ising Coupling via Numerical Basis Optimization]].

## When to reach for it

- Compiling a [[Unitary Coupled Cluster (UCC) Ansatz|UCC doubles]] operator into a circuit with minimal depth on a linearly-connected qubit array.
- Any time you need to implement $e^G$ for a two-body generator $G$ and want the fewest interleaved Givens/Ising layers.
- Classical preprocessing for near-term quantum chemistry: the $O(n^5)$ iteration cost is acceptable for systems up to ~100 spin-orbitals.
- Can extend to higher-body operators with iteration cost matching the single-particle rotation cost of the higher-rank tensor.

## Complexity

- **Per iteration:** $O(n^5)$ for the gradient computation (same as a single basis transformation of the two-body tensor). L-BFGS or similar optimizer used.
- **Number of iterations:** Problem-dependent. For UCC doubles, typically $L \sim \text{rank}(\tilde{A})/4$ factors needed for sub-milliHartree accuracy, where $\tilde{A}$ is the coefficient supermatrix.
- **Circuit cost per factor:** $O(n^2)$ gates, $O(n)$ depth (one Givens layer + one Ising layer).
- **Total circuit depth:** $O(Ln)$.

## Caveat

The residual rank rises to maximum during greedy extraction, causing convergence slowdown at high accuracy. The first few factors capture most of the correlation energy, but squeezing out the last bits gets progressively harder. For practical near-term use (sub-milliHartree is usually fine), this isn't a problem. For exact decomposition, consider switching to Takagi on the residual after the greedy phase saturates.

Also: the greedy procedure has no global optimality guarantee. Empirically, random initialization and Takagi-seeded initialization give similar results, but there's no proof you've found the best decomposition.

Does **not** work well for compressing Hamiltonians directly (the objective maximizes diagonal weight, not spectral fidelity).

## Related notes
- [[Compressing Many-Body Fermion Operators under Unitary Constraints (Rubin, Lee, Babbush 2021) — Paper Notes]]
- [[Full-Rank Ising Coupling via Numerical Basis Optimization]]
- [[Compressed UCC as ADAPT-VQE Initial State]]
- [[Givens Rotation Slater Determinant Preparation]]
- [[Fermionic Swap Network]]
- [[Unitary Coupled Cluster (UCC) Ansatz]]
- [[Trotterized Time Evolution for Chemistry]]
