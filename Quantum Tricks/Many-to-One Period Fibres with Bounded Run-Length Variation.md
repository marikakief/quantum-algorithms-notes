# Many-to-One Period Fibres with Bounded Run-Length Variation

> **Source:** Arthur Schmidt, arXiv:0912.4807
> **Tags:** #trick #period-finding #hidden-subgroup-problem #number-theory

## What it does

Lets QFT period finding work even when the sampled function is many-to-one inside each period, provided the repeated fibres have controlled shapes across periods.

## The trick

Standard period finding is cleanest when a measured function value leaves a uniform coset state
$$
\sum_k |x_0+kR\rangle.
$$
Schmidt's regulator function instead leaves blocks:
$$
\sum_{k\in M}\sum_{m=0}^{m(x_0,k)} |x_0+4kR^+ + m + \epsilon(x_0,k)\rangle,
$$
where a single function value occurs over a short run of consecutive integers in each period.

The Fourier argument still works if the block lengths do not drift too much. In the real-quadratic regulator algorithm, the paper proves
$$
1\le m(g,k)<\ln\Delta+3,
\qquad
\max_{k,k'}|m(g,k)-m(g,k')|\le 4.
$$
For measured frequencies near integer multiples of $q/R^+$, all phases from the block lie in an arc of length at most $\pi/2$. The amplitudes therefore add with a constant fraction of full constructive interference, giving constant success probability rather than being washed out by the many-to-one fibres.

## When to reach for it

- Period-finding reductions where computing a one-to-one representative inside a period costs extra registers.
- Continuous or discretised period functions with boundary ambiguity.
- Number-theoretic infrastructure algorithms where several nearby inputs naturally map to the same reduced object.

## Complexity

The trick usually trades injectivity for a proof obligation: show bounded fibre width and bounded fibre variation across periods. If the maximum run length is $M$ and the variation is $O(1)$, the QFT success probability loses only a constant or mild $1/M$ factor depending on the sampling window.

In Schmidt's regulator case, the success probability of Regulator-Dual remains at least $2^{-11}$.

## Caveat

Many-to-one by itself is not enough. If the fibre pattern changes erratically from one period to the next, the post-measurement state can have phase noise that kills the Fourier peak. The useful condition is not injectivity; it is stable fibre geometry.

## Related notes

- [[Quantum Algorithms for Many-to-One Functions to Solve the Regulator and the Principal Ideal Problem (Schmidt 2009) — Paper Notes]]
- [[Period Finding for Irrational Periods on R]]
- [[Interference-Based Period Finding]]
- [[HSP as Algorithmic Template (Abelian)]]
