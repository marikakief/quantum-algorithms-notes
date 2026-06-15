# GRH Expander Smoothing for Class-Group Actions

> **Source:** Childs--Jao--Soukharev, arXiv:1012.4019
> **Tags:** #trick #class-groups #isogenies #number-theory #smoothing

## What it does

Finds a smooth representative of an arbitrary ideal class using expander mixing of small split-prime Cayley graphs, rather than assuming random smoothness heuristics.

## The trick

Let $G=\operatorname{Cl}(\mathcal O_\Delta)$ and let $S_x$ be the multiset of ideal classes of split prime ideals with norm at most $x=L(z)$, together with inverses. Under GRH, the Cayley graph
$$
\operatorname{Cay}(G,S_x)
$$
mixes fast enough that a random walk of length
$$
t\ge C\frac{\log |G|}{\log\log |\Delta|}
$$
lands in any set $S\subseteq G$ with probability at least
$$
\frac12\frac{|S|}{|G|}.
$$

To smooth a target class $[\mathfrak b]$, sample short products $\mathcal F^v$ over the factor base $\mathcal F$ and reduce an ideal in
$$
[\mathfrak b][\mathcal F^v].
$$
If the reduced ideal has $\mathcal F$-smooth norm, write it as $\mathcal F^a$ and return the relation
$$
[\mathfrak b]=[\mathcal F^{a-v}].
$$
The expander theorem supplies a lower bound on the chance of hitting the smooth subset of reduced ideals.

## When to reach for it

- Class-group algorithms where arbitrary ideal classes must be rewritten over small prime ideals.
- Isogeny-action evaluation where the final computation needs a composition of small-degree isogenies.
- Any setting where a heuristic "random reduced ideal is smooth often enough" needs a GRH-backed replacement.

## Complexity

In [[Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) — Paper Notes]], the relation-finding step runs in
$$
L(z)+L\!\left(\frac{1}{4z}\right),
$$
and succeeds with probability at least $1-1/e$ after
$$
L\!\left(\frac{1}{4z}\right)
$$
samples. The returned exponent vector has
$$
\|a-v\|_1 < (C+1)\log |\Delta|,
$$
which keeps the later isogeny composition short.

## Caveat

The guarantee depends on GRH and on having the right class-group/Cayley-graph setting. It is not a general-purpose smoothness theorem for arbitrary arithmetic objects. The method may also be slower in practice than heuristic exponent-distribution methods because it avoids assumptions those methods rely on.

## Related notes

- [[Constructing Elliptic Curve Isogenies in Quantum Subexponential Time (Childs-Jao-Soukharev 2014) — Paper Notes]]
- [[Injective Group-Action Hidden Shift from Principal Homogeneous Spaces]]
- [[Polynomial-Time Quantum Algorithms for Pell's Equation and the Principal Ideal Problem (Hallgren 2002) — Paper Notes]]
