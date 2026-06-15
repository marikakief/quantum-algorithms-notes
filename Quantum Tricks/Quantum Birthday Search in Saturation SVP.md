# Quantum Birthday Search in Saturation SVP

> **Source:** Laarhoven, Mosca, van de Pol, arXiv:1301.6176
> **Tags:** #trick #quantum-search #lattice-saturation #shortest-vector-problem

## What it does

Combines saturation-list sampling with Grover search over reduction lists and close pairs, lowering the exponent while keeping the birthday-paradox success argument.

## The trick

A saturation SVP algorithm builds enough short lattice vectors that two of them are likely to be closer than the target bound $\mu$. The Pujol--Stehlé version separates the proof into two lists:

1. a dummy reduction list $T$, used to reduce perturbed samples;
2. a fresh sample list $S$, whose elements are independent enough for a birthday argument.

The final step searches for

$$
(s_1,s_2)\in S\times S
\quad\text{such that}\quad
0<\|s_1-s_2\|<\mu.
$$

Classically, there are three linear-list bottlenecks:

- reducing samples while building $T$;
- reducing samples against $T$ while building $S$;
- searching $S\times S$ for the close pair.

Replace each with [[A Fast Quantum Mechanical Algorithm for Database Search (Grover 1996) — Paper Notes|Grover search]]. If the classical close-pair search costs $\widetilde{O}(|S|^2)$, the quantum search over the product list costs

$$
\widetilde{O}(\sqrt{|S|^2})=\widetilde{O}(|S|).
$$

The useful point is that the success proof is not replaced by quantum reasoning. The birthday-paradox guarantee supplies marked items in $S\times S$; Grover search only finds one faster.

## When to reach for it

Reach for this when a classical algorithm deliberately over-samples until a collision or close pair appears, then scans a product space to find it. Good candidates:

- saturation algorithms;
- birthday-paradox collision steps;
- list-merging attacks where the proof gives many hidden pair witnesses;
- geometric algorithms where the witness predicate is a distance or norm test.

## Complexity

In the notation used by Laarhoven--Mosca--van de Pol for Pujol--Stehlé saturation, the quantum time exponent is

$$
\tilde t=\max\left(c_g+\frac{3c_t}{2},\; c_g+\frac{c_b}{2}+\frac{c_t}{2},\; c_g+\frac{c_b}{2}\right),
$$

and the space exponent is

$$
\tilde s=\max\left(c_t,\; c_g+\frac{c_b}{2}\right).
$$

Optimising their constants gives exact SVP in time $2^{1.799n+o(n)}$ and space $2^{1.286n+o(n)}$.

## Caveat

The product-space search still needs indexed access to $S$ and reversible evaluation of the close-pair predicate. Also, if the classical birthday step only gives a very small success probability, the Grover cost must include the usual repetition or unknown-solution handling.

## Related notes

- [[Solving the Shortest Vector Problem in Lattices Faster Using Quantum Search (Laarhoven-Mosca-van de Pol 2013) — Paper Notes]]
- [[Groverised Centre Search in Lattice Sieves]]
- [[QRACM Accounting for Quantum Search over Classical Lists]]
- [[Birthday-Grover Space-Time Tradeoff]]
