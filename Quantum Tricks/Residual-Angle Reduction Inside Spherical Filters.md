# Residual-Angle Reduction Inside Spherical Filters

> **Source:** Chailloux--Loyer, arXiv:2105.05608
> **Tags:** #trick #lattice-sieving #locality-sensitive-filtering #geometry

## What it does

Turns reducing-pair detection inside one spherical filter into a smaller-dimensional angle test on residual vectors.

## The trick

Suppose two unit vectors $\vec x_0,\vec x_1$ survive a hypercone filter with centre $\vec s$ and angle $\alpha$. In high dimension, most surviving vectors lie near the cap boundary, so write
$$
\vec x_i=\cos(\alpha)\vec s+\sin(\alpha)\vec y_i,
$$
where $\vec y_i\perp \vec s$ and $\|\vec y_i\|=1$.

Then the condition that the original pair is reducing,
$$
\theta(\vec x_0,\vec x_1)\leq \pi/3,
$$
is equivalent to
$$
\theta(\vec y_0,\vec y_1)\leq
\theta^*_\alpha:=2\arcsin\!\left(\frac{1}{2\sin\alpha}\right).
$$
The proof is just subtract-and-square:
$$
\|\vec x_0-\vec x_1\|^2
=\sin^2(\alpha)\|\vec y_0-\vec y_1\|^2
=\sin^2(\alpha)(2-2\cos\theta_y).
$$
Asking $\|\vec x_0-\vec x_1\|\leq1$ gives the displayed threshold for $\theta_y$.

## When to reach for it

Use it when a nearest-neighbour or pair-finding task has already bucketed points by spherical caps and the next operation depends only on pairwise closeness inside one bucket. The residual view separates the common filter-centre direction from the actual degrees of freedom that decide reducibility.

## Complexity

No asymptotic overhead beyond computing/storing the residual representation. The gain is analytic: it gives the right residual cap volume $V_d(\theta^*_\alpha)$ for counting reducing pairs and designing inner filters.

## Caveat

The boundary representation is heuristic in the same sense as lattice-sieving volume estimates: it relies on high-dimensional cap mass concentrating near angle $\alpha$. It is not a worst-case geometric statement about arbitrary bucket contents.

## Related notes

- [[Lattice Sieving via Quantum Random Walks (Chailloux-Loyer 2021) — Paper Notes]]
- [[Local-Filter Johnson Walk for Bucket Pair Finding]]
- [[Groverised Centre Search in Lattice Sieves]]
