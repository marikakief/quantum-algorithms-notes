# Monomial-Subset Coupon Collection for Boolean Solution Readout

> **Source:** Ding--Gheorghiu--Gilyén--Hallgren--Li, arXiv:2111.00405
> **Tags:** #trick #readout #HHL #coupon-collector #boolean-systems

## What it does

Recovers a Boolean solution support by measuring monomial labels from a QLSA output state, using subset coupon collection instead of coordinate tomography.

## The trick

Suppose a Boolean polynomial system has a unique solution $a\in\{0,1\}^n$. Let
$$
S=\{x_i:a_i=1\}.
$$
For the Boolean Macaulay linear system, the monomial solution vector has nonzero entries exactly on multilinear monomials supported inside $S$.

If the matrix degree is $d$, the QLSA output state is ideally
$$
|y\rangle=\frac{1}{\sqrt{|S_d|}}
\sum_{R\in S_d}|R\rangle,
$$
where
$$
S_d=\{R\subseteq S:1\le |R|\le d\}.
$$
A computational-basis measurement gives a uniformly random nonempty subset $R\subseteq S$ of size at most $d$. Taking the union of measured subsets gradually recovers $S$.

For each $x\in S$, the probability that one sample contains $x$ is
$$
\frac{\sum_{i=1}^d \binom{|S|-1}{i-1}}
{\sum_{i=1}^d \binom{|S|}{i}}.
$$
The paper proves that
$$
r=O\!\left(\frac{|S|}{d}\log\frac{|S|}{\epsilon}\right)
$$
samples suffice to recover all of $S$ with probability at least $1-\epsilon$.

## When to reach for it

Use this when a quantum linear-system solver gives a state whose basis labels encode **sets of true variables** or other correlated witness fragments. It is a way around the usual “QLSA gives a state, not a classical answer” readout problem.

## Complexity

The quantum cost is $r$ preparations of the QLSA solution state, plus computational-basis measurements. The classical postprocessing is just taking unions of sampled subsets. For $d=n$ and a nonempty $S$, only $O(\log |S|)$ samples are needed up to constants.

## Caveat

This works because the support pattern is very special: nonzero amplitudes correspond exactly to subsets of the unknown support. Generic QLSA output states do not have this structure. Approximate QLSA output also requires the state error small enough across all $r$ samples; the paper takes per-copy error $O(1/r)$.

## Related notes

- [[Limitations of the Macaulay Matrix Approach for Using the HHL Algorithm to Solve Multivariate Polynomial Systems (Ding-Gheorghiu-Gilyen-Hallgren-Li 2023) — Paper Notes]]
- [[Boolean Solution Extraction by Monomial Measurement]]
- [[Boolean Macaulay Reduction over C]]
- [[Macaulay-HHL Pseudo-Solution States]]
