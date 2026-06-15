# Coefficient-Preserving Random BDD Self-Reduction

> **Source:** Eldar--Hallgren, arXiv:2201.13450
> **Tags:** #trick #lattice-problems #bounded-distance-decoding #random-self-reduction #finite-abelian-groups

## What it does

Turns noisy random linear information about the hidden closest-vector coefficient into a lower-dimensional random BDD instance with the same coefficient solution.

## The trick

Let $G\leq\mathbb{Z}_q^n$ be a finite quotient of a $q$-periodic lattice, and suppose the BDD target has closest group element

$$
Gs\in G.
$$

A sampling subroutine returns random $a_i\in\mathbb{Z}_q^r$ and noisy values

$$
O_i\approx (-s)\cdot a_i\pmod q.
$$

Stack the random $a_i$ as rows of a matrix

$$
\widetilde G\in\mathbb{Z}_q^{m\times r},
$$

and set

$$
\widetilde t=(O_1,
\ldots,O_m)
\in\mathbb{Z}_q^m.
$$

Then the vector $\widetilde Gs$ is close to $\widetilde t$. With high probability over the random rows, $\widetilde G$ defines a random $q$-ary lattice with a sufficiently long shortest vector and primitive enough column span. The same coefficient vector $s$ is then the unique closest-vector coefficient in the reduced BDD instance.

Eldar--Hallgren bound the distance as

$$
\operatorname{dist}_q(\widetilde t,\widetilde G)
\leq
\sqrt{\epsilon_1}\,q^{r/m}\lambda_1(\widetilde G)\,260n^{3/4}m^{2.5}p_{\mathrm{err}}^{-2}.
$$

Choosing $m\approx\sqrt{r\log q}$ makes the random instance low-dimensional enough for Babai/LLL while still preserving enough distance slack.

## When to reach for it

Use this pattern when:

- the unknown object is a coefficient vector $s$ over a finite abelian group;
- the quantum subroutine can sample noisy random linear measurements of $s$;
- random instances in a lower-dimensional family have better geometric guarantees than the original worst-case instance;
- solving the reduced instance reveals the original coefficient vector.

## Complexity

The reduction uses $m$ samples. In the polynomial-time Eldar--Hallgren setting,

$$
m=\sqrt{r\log q},
$$

and the full algorithm runs in $\operatorname{poly}(n,\log q)$ time for BDD approximation factor

$$
2^{-\Omega(\sqrt{r\log q})}.
$$

With Schnorr-style post-processing, one can trade time $\beta^\beta$ for a stronger approximation regime.

## Caveat

The reduction is only as good as the noise in the sampled inner products. If approximate phase estimation produces errors above the random lattice's decoding radius, the coefficient $s$ is no longer recoverable. The argument also uses random $q$-ary lattice shortest-vector estimates and primitivity bounds.

## Related notes

- [[An Efficient Quantum Algorithm for Lattice Problems Achieving Subexponential Approximation Factor (Eldar-Hallgren 2022) — Paper Notes]]
- [[Phased Cube States as Approximate Shift Eigenvectors]]
- [[Approximate-Eigenvector Phase Estimation for Noisy Modular Inner Products]]
- [[Quantum Computation and Lattice Problems (Regev 2002) — Paper Notes]]
