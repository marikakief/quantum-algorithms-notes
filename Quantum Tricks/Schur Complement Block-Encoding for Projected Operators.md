# Schur Complement Block-Encoding for Projected Operators

> **Source:** Hayakawa, arXiv:2111.00433 (2022)
> **Tags:** #trick #block-encoding #QSVT #Schur-complement

## What it does
Block-encodes a Schur complement $M/D = A - BD^+C$ by composing block-encodings of the constituent blocks and a QSVT-based pseudo-inverse.

## The trick
Given a block matrix $M = \begin{pmatrix} A & B \\ C & D \end{pmatrix}$, the Schur complement $M/D = A - BD^+C$ can be block-encoded by:

1. Block-encode $D$ using boundary operators and membership oracles
2. Use [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes|QSVT]] with a polynomial approximation of $1/x$ to implement $D^+$ as a $(\kappa, a+1, \epsilon)$-block-encoding, where $\kappa = \alpha/\lambda_{\min}(D)$
3. Block-encode $B D^+ C$ by composing the block-encodings of $B$, $D^+$, and $C$ (product of block-encodings)
4. Block-encode $A - BD^+C$ via addition of block-encodings

The pseudo-inverse QSVT uses degree $d = O(\kappa \log(\kappa/\epsilon))$.

## When to reach for it
- Persistent Laplacian construction (the "up" part is a Schur complement of the ordinary Laplacian)
- Any operator defined by projecting out a subspace from a larger operator
- Constrained optimisation where effective Hamiltonians arise from eliminating degrees of freedom

## Complexity
Total cost: $O(d)$ uses of the block-encoding of $D$, plus the costs of block-encoding $A$, $B$, $C$. The pseudo-inverse degree $d = O(\kappa \log(\kappa/\epsilon))$ depends on the condition number $\kappa$ of $D$.

## Caveat
Requires $D$ to have an inverse-polynomial spectral gap (otherwise $\kappa$ blows up and the QSVT polynomial degree becomes infeasible). This introduces the additional spectral gap assumption ($\gamma_{\min}$) seen in [[Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) — Paper Notes|Hayakawa (2022)]].

## Related notes
- [[Quantum Algorithm for Persistent Betti Numbers (Hayakawa 2022) — Paper Notes]]
- [[QSVT and Beyond (Gilyén et al. 2018-2019) — Paper Notes]]
- [[Persistent Laplacian for Tracking Topological Persistence]]
