# Hadamard Product Spectral Norm Bound

> **Source:** Mathias (1990), applied in Špalek-Szegedy, arXiv:quant-ph/0409116
> **Tags:** #trick #linear-algebra #spectral-norm #adversary-method #query-complexity

## What it does
Bounds the spectral norm of a Hadamard (entry-wise) product of non-negative matrices in terms of row and column norms of factor matrices, providing the bridge between spectral and weighted adversary formulations.

## The trick

**Lemma (Mathias 1990):** Let $S$ be a non-negative symmetric matrix with $S \leq M \circ N$ entry-wise, where $M, N \geq 0$. Then:

$$\lambda(S) \leq r(M) \cdot c(N)$$

where $r(M) = \max_x \|M_x\|_2$ (max row $\ell_2$-norm) and $c(N) = \max_y \|N^y\|_2$ (max column $\ell_2$-norm).

Moreover, for any symmetric $S \geq 0$ there exists an optimal factorisation $S = M \circ M^T$ with $r(M) = c(M^T) = \sqrt{\lambda(S)}$, obtained by setting $M[x,y] = \sqrt{S[x,y]} \cdot d[y]/d[x]$ where $d$ is the principal eigenvector.

**Strengthened version (Špalek-Szegedy):** The bound tightens to:

$$\lambda(S) \leq \max_{\substack{x,y \\ S[x,y] > 0}} r_x(M) \cdot c_y(N)$$

restricting the max to entries where $S$ is non-zero.

## When to reach for it

- Converting a spectral adversary bound into a weighted adversary weight scheme: decompose $\Gamma \circ D_i$ into $M_i \circ M_i^T$ and read off the weights $w'(x,y,i) = M_i[x,y]^2 \cdot \delta[x]^2$
- Any situation where you need to bound the spectral norm of an entry-wise product and direct eigenvalue computation is intractable
- Bounding operator norms that arise from masking matrices (zeroing out entries based on some condition)

## Complexity
The optimal factorisation requires computing the principal eigenvector of $S$ — $O(|S|)$ per power iteration step.

## Caveat
The bound is for non-negative matrices only. Negative entries require different techniques. The strengthened version's proof (Appendix A of Špalek-Szegedy) is a case analysis that carefully perturbs entries to move the norm-achieving rows/columns into the support of $S$.

## Related notes
- [[All Quantum Adversary Methods Are Equivalent (Špalek-Szegedy 2006) — Paper Notes]]
- [[SDP Duality for Query Complexity Bounds]]
