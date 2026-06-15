# Chevalley-Warning Search for Nil-2 HSP Actions

## Pattern

Use Chevalley-Warning-style existence of nonzero finite-field solutions as an algorithmic guide for choosing action labels that cancel phases in a nil-2 HSP algorithm.

## Problem shape

In the nil-2 HSP algorithm, phase cancellation requires a nonzero vector

$$j=(j_1,\ldots,j_n) \in \mathbb Z_p^n$$

satisfying

$$\sum_i u_i j_i^2 = 0_d,\qquad \sum_i u_i j_i = 0_d,$$

where each $u_i \in \mathbb Z_p^d$ labels a Fourier character of the central subgroup $G'$.

The quadratic constraints cancel central phases. The linear constraints cancel the remaining subgroup-dependent phases after using the automorphism formula.

## The trick

Chevalley-Warning says that a homogeneous system over a finite field has nontrivial solutions when the number of variables is large enough relative to total degree. Ivanyos-Sanselme-Santha do not stop at existence: they choose

$$n = \frac{(d+1)^2(d+2)}{2}$$

and give a polynomial-time search procedure.

## Search outline

1. Split variables into $d+1$ blocks.
2. In each block, solve the $d$ homogeneous quadratic equations.
3. Use row operations to reduce the quadratic system by induction on $d$.
4. Use quadratic-residue testing and modular square roots to satisfy the remaining binary/ternary quadratic pieces.
5. Combine the block solutions with coefficients $\lambda_0,\ldots,\lambda_d$.
6. Solve the residual $d$ linear equations in the $d+1$ block coefficients to get a nonzero combined solution.

## Where it appears

- [[An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Nil-2 Groups (Ivanyos-Sanselme-Santha 2008) — Paper Notes|Ivanyos-Sanselme-Santha (2008)]].
- It generalises the smaller witness search from [[An Efficient Quantum Algorithm for the Hidden Subgroup Problem in Extraspecial Groups (Ivanyos-Sanselme-Santha 2007) — Paper Notes|Ivanyos-Sanselme-Santha (2007)]], where $d=1$ and four copies suffice with constant success probability.

## Why it is useful

It turns an existence theorem for finite-field polynomial systems into a concrete subroutine inside a quantum algorithm. The lesson is broader than HSP: if quantum phases are polynomial functions over a finite field, phase cancellation can sometimes be reduced to solving low-degree polynomial systems.

## Failure modes

- The required number of copies may grow too large if the central dimension is large in the wrong input model.
- Chevalley-Warning gives existence, but an efficient search method may still be hard for other polynomial systems.
- The method depends on finite-field arithmetic and on homogeneous low-degree constraints.

## Related cards

- [[Automorphism-Action Reduction to Abelian HSP]]
- [[HSP as Algorithmic Template (Abelian)]]
