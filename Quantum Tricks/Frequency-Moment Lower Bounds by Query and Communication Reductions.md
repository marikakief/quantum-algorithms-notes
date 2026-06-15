# Frequency-Moment Lower Bounds by Query and Communication Reductions

> **Source:** Montanaro, arXiv:1505.00113
> **Tags:** #trick #lower-bound #frequency-moments #query-complexity #communication-complexity

## What it does

Pins down frequency-moment lower bounds by embedding standard hard problems into small changes of $F_k$.

## The trick

Choose an encoding where the hard problem changes the multiplicity profile just enough that a relative-error estimate of $F_k$ distinguishes the cases.

Query reductions:

- **Threshold to $F_0$:** encode bit $x_i=1$ as a unique value and $x_i=0$ as a common dummy value. Then $F_0$ tracks Hamming weight.
- **Approximate counting to $F_k$:** encode ones as a high-multiplicity dummy value and zeros as unique values. A small Hamming-weight gap becomes a relative gap in $F_k$.
- **Collision / element distinctness:** compare one-to-one versus two-to-one inputs; $F_k$ changes from $n$ to $n2^{k-1}$.

Streaming reductions:

- **Equality:** construct set streams whose moment differs by a constant factor depending on whether Alice and Bob's inputs match.
- **Disjointness:** common elements make $F_\infty\ge2$ instead of $1$.
- **Gap-Hamming:** known reductions from frequency moments to GHD plus quantum communication lower bounds yield $TS=\Omega(1/\epsilon)$.

## When to reach for it

- Lower-bounding moment, norm, or multiplicity-estimation problems.
- Showing that high-precision moment estimation becomes full input recovery.
- Translating streaming algorithms into communication protocols via pass simulation.

## Complexity

Montanaro obtains, among others,

$$
Q(F_0)=\Omega(\sqrt{n/\epsilon}),\quad Q(F_k)=\Omega(n^{1/2-1/(2k)}/\epsilon),\quad Q(F_\infty)=\Omega(n^{2/3}),
$$

and quantum streaming

$$
TS=\Omega(1/\epsilon)
$$

for $k\ne1$.

## Caveat

Some bounds are not tight for all $k$ and $\epsilon$. In particular, the query complexity of approximating $F_\infty$ remains open beyond the reductions through gapped $k$-distinctness.

## Related notes

- [[Quantum Complexity of Approximating the Frequency Moments (Montanaro 2016) — Paper Notes]]
- [[Quantum Lower Bounds by Polynomials (Beals-Buhrman-Cleve-Mosca-de Wolf 1998) — Paper Notes]]
- [[Quantum Walk Algorithm for Element Distinctness (Ambainis 2007) — Paper Notes]]
- [[The Quantum Query Complexity of Approximating the Median (Nayak-Wu 1999) — Paper Notes]]
